# SPDX-License-Identifier: LGPL-2.1-or-later

units = [
        { 'file' : 'app.slice' },
        { 'file' : 'background.slice' },
        { 'file' : 'basic.target' },
        { 'file' : 'bluetooth.target' },
        { 'file' : 'capsule@.target' },
        { 'file' : 'default.target' },
        { 'file' : 'exit.target' },
        { 'file' : 'graphical-session-pre.target' },
        { 'file' : 'graphical-session.target' },
        { 'file' : 'paths.target' },
        { 'file' : 'printer.target' },
        { 'file' : 'session.slice' },
        { 'file' : 'shutdown.target' },
        { 'file' : 'smartcard.target' },
        { 'file' : 'sockets.target' },
        { 'file' : 'sound.target' },
        {
          'file' : 'systemd-ask-password.socket',
          'symlinks' : ['sockets.target.wants/']
        },
        { 'file' : 'systemd-ask-password@.service' },
        { 'file' : 'systemd-exit.service' },
        { 'file' : 'systemd-tmpfiles-clean.service' },
        { 'file' : 'systemd-tmpfiles-clean.timer' },
        { 'file' : 'systemd-tmpfiles-setup.service' },
        { 'file' : 'timers.target' },
        {
          'file' : 'xdg-desktop-autostart.target',
          'conditions': ['ENABLE_XDG_AUTOSTART'],
        }
]

foreach unit : units
        file = unit.get('file')

        install = true
        foreach cond : unit.get('conditions', [])
                if conf.get(cond) != 1
                        install = false
                        break
                endif
        endforeach

        if install
                install_data(file,
                             install_dir : userunitdir)

                foreach target : unit.get('symlinks', [])
                        if target.endswith('/')
                                # '/' is only allowed at the end of the target
                                assert(target.replace('/', '') + '/' == target)
                                install_symlink(file,
                                                pointing_to : '..' / file,
                                                install_dir : userunitdir / target)
                        else
                                install_symlink(target,
                                                pointing_to : file,
                                                install_dir : userunitdir)
                        endif
                endforeach
        endif
endforeach
