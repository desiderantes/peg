project('peg', 'c',
  version: '0.1.20',
  default_options: [
    'warning_level=1',
    'c_std=gnu99',
    'optimization=3'
  ]
)

cc = meson.get_compiler('c')

c_args = []
inc_dirs = [include_directories('src')]

# OS-specific compatibility layers for Windows
if host_machine.system() == 'windows'
  inc_dirs += include_directories('win')
  c_args += ['-DWIN32', '-D_WINDOWS', '-D_CRT_SECURE_NO_WARNINGS']
else
  c_args += ['-D_GNU_SOURCE', '-D_POSIX_C_SOURCE=200809L']
endif

subdir('src')
subdir('examples')


if get_option('emacs')
  install_data('leg-mode.el', install_dir: get_option('datadir') / 'emacs' / 'site-lisp')
endif

