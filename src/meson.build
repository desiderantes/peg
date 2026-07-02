version_arr = meson.project_version().split('.')

version_header = configure_file(
  output: 'version.h',
  configuration: {
    'PEG_MAJOR': version_arr[0].to_int(),
    'PEG_MINOR': version_arr[1].to_int(),
    'PEG_LEVEL': version_arr[2].to_int()
  }
)

peg_sources = files(
  'peg.c',
  'tree.c',
  'compile.c',
) + version_header

leg_sources = files(
  'leg.c',
  'tree.c',
  'compile.c',
) + version_header

# OS-specific adjustments on Windows
if host_machine.system() == 'windows'
  peg_sources += files('../win/getopt.c')
  leg_sources += files('../win/getopt.c')
endif

peg_exe = executable('peg',
  peg_sources,
  include_directories: inc_dirs,
  c_args: c_args,
  install: true
)

leg_exe = executable('leg',
  leg_sources,
  include_directories: inc_dirs,
  c_args: c_args,
  install: true
)

# Install man pages
install_man('peg.1')

# Bootstrap and self-hosting verification tests (completely Python-free)
sh = find_program('sh', required: false)

if sh.found()
  test('check-peg', sh,
    args: [
      '-c',
      '"@0@" -o temp_peg.peg-c "@1@" && grep -v "^#line" temp_peg.peg-c > temp_peg_t1 && grep -v "^#line" "@2@" > temp_peg_t2 && diff -u temp_peg_t2 temp_peg_t1 && rm -f temp_peg_t1 temp_peg_t2 temp_peg.peg-c'.format(
        peg_exe.full_path(),
        meson.current_source_dir() / 'peg.peg',
        meson.current_source_dir() / 'peg.peg-c'
      )
    ],
    depends: [peg_exe],
    workdir: meson.current_build_dir()
  )

  test('check-leg', sh,
    args: [
      '-c',
      '"@0@" -o temp_leg.c "@1@" && grep -v "^#line" temp_leg.c > temp_leg_t1 && grep -v "^#line" "@2@" > temp_leg_t2 && diff -u temp_leg_t2 temp_leg_t1 && rm -f temp_leg_t1 temp_leg_t2 temp_leg.c'.format(
        leg_exe.full_path(),
        meson.current_source_dir() / 'leg.leg',
        meson.current_source_dir() / 'leg.c'
      )
    ],
    depends: [leg_exe],
    workdir: meson.current_build_dir()
  )

  # testleg self-hosting verification test
  testleg_c = custom_target('testleg_c',
    input: 'leg.leg',
    output: 'testleg.c',
    command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
  )
  testleg_exe = executable('testleg',
    sources: [testleg_c, 'compile.c', 'tree.c'],
    include_directories: inc_dirs,
    c_args: c_args
  )
  test('testleg', sh,
    args: [
      '-c',
      '"@0@" -o temp_testleg.c.c "@1@" && grep -v "^#line" temp_testleg.c.c > temp_testleg_t1 && grep -v "^#line" "@2@" > temp_testleg_t2 && diff -u temp_testleg_t2 temp_testleg_t1 && rm -f temp_testleg_t1 temp_testleg_t2 temp_testleg.c.c'.format(
        testleg_exe.full_path(),
        meson.current_source_dir() / 'leg.leg',
        testleg_c.full_path()
      )
    ],
    depends: [testleg_exe],
    workdir: meson.current_build_dir()
  )

  run_target('new',
    command: [
      sh, '-c',
      '"$1" -o "$2" "$3" && "$4" -o "$5" "$6"',
      'sh',
      peg_exe.full_path(),
      meson.current_source_dir() / 'peg.peg-c',
      meson.current_source_dir() / 'peg.peg',
      leg_exe.full_path(),
      meson.current_source_dir() / 'leg.c',
      meson.current_source_dir() / 'leg.leg'
    ],
    depends: [peg_exe, leg_exe]
  )
endif
