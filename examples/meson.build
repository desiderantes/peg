examples_inc = [include_directories('../src'), include_directories('.')]
if host_machine.system() == 'windows'
  examples_inc += include_directories('../win')
endif

# Helper command to run peg to generate .c file and touch dummy .h file for dependency tracking without compilation
sh = find_program('sh', required: true)

# Examples definitions
# 1. test
test_peg_files = custom_target('test_peg_files',
  input: 'test.peg',
  output: ['test.peg.c', 'test.peg.h'],
  command: [
    sh, '-c',
    '"$1" -o "$2" "$3" && touch "$4"',
    'sh',
    peg_exe,
    '@OUTPUT0@',
    '@INPUT@',
    '@OUTPUT1@'
  ]
)
test_example_exe = executable('test_example',
  sources: ['test.c', test_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 2. rule
rule_peg_files = custom_target('rule_peg_files',
  input: 'rule.peg',
  output: ['rule.peg.c', 'rule.peg.h'],
  command: [
    sh, '-c',
    '"$1" -o "$2" "$3" && touch "$4"',
    'sh',
    peg_exe,
    '@OUTPUT0@',
    '@INPUT@',
    '@OUTPUT1@'
  ]
)
rule_example_exe = executable('rule_example',
  sources: ['rule.c', rule_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 3. accept
accept_peg_files = custom_target('accept_peg_files',
  input: 'accept.peg',
  output: ['accept.peg.c', 'accept.peg.h'],
  command: [
    sh, '-c',
    '"$1" -o "$2" "$3" && touch "$4"',
    'sh',
    peg_exe,
    '@OUTPUT0@',
    '@INPUT@',
    '@OUTPUT1@'
  ]
)
accept_example_exe = executable('accept_example',
  sources: ['accept.c', accept_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 4. wc
wc_leg_c = custom_target('wc_leg_c',
  input: 'wc.leg',
  output: 'wc.leg.c',
  command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
)
wc_example_exe = executable('wc_example',
  sources: [wc_leg_c],
  include_directories: examples_inc,
  c_args: c_args
)

# 5. dc
dc_peg_files = custom_target('dc_peg_files',
  input: 'dc.peg',
  output: ['dc.peg.c', 'dc.peg.h'],
  command: [
    sh, '-c',
    '"$1" -o "$2" "$3" && touch "$4"',
    'sh',
    peg_exe,
    '@OUTPUT0@',
    '@INPUT@',
    '@OUTPUT1@'
  ]
)
dc_example_exe = executable('dc_example',
  sources: ['dc.c', dc_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 6. dcv
dcv_peg_files = custom_target('dcv_peg_files',
  input: 'dcv.peg',
  output: ['dcv.peg.c', 'dcv.peg.h'],
  command: [
    sh, '-c',
    '"$1" -o "$2" "$3" && touch "$4"',
    'sh',
    peg_exe,
    '@OUTPUT0@',
    '@INPUT@',
    '@OUTPUT1@'
  ]
)
dcv_example_exe = executable('dcv_example',
  sources: ['dcv.c', dcv_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 7. calc
calc_leg_c = custom_target('calc_leg_c',
  input: 'calc.leg',
  output: 'calc.leg.c',
  command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
)
calc_example_exe = executable('calc_example',
  sources: [calc_leg_c],
  include_directories: examples_inc,
  c_args: c_args
)

# 8. basic
basic_leg_c = custom_target('basic_leg_c',
  input: 'basic.leg',
  output: 'basic.leg.c',
  command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
)
basic_example_exe = executable('basic_example',
  sources: [basic_leg_c],
  include_directories: examples_inc,
  c_args: c_args
)

# 9. localpeg (uses test_peg_files[1] for dependency)
localpeg_example_exe = executable('localpeg_example',
  sources: ['localpeg.c', test_peg_files[1]],
  include_directories: examples_inc,
  c_args: c_args
)

# 10. localleg
localleg_leg_c = custom_target('localleg_leg_c',
  input: 'localleg.leg',
  output: 'localleg.leg.c',
  command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
)
localleg_example_exe = executable('localleg_example',
  sources: [localleg_leg_c],
  include_directories: examples_inc,
  c_args: c_args
)

# 11. erract
erract_leg_c = custom_target('erract_leg_c',
  input: 'erract.leg',
  output: 'erract.leg.c',
  command: [leg_exe, '-o', '@OUTPUT@', '@INPUT@']
)
erract_example_exe = executable('erract_example',
  sources: [erract_leg_c],
  include_directories: examples_inc,
  c_args: c_args
)

# Example tests run using POSIX shell commands
if sh.found()
  # 1. test
  test('test', sh,
    args: ['-c', 'echo "ab.ac.ad.ae.afg.afh.afg.afh.afi.afj." | "$1" | diff -u "$2" -', 'sh', test_example_exe.full_path(), meson.current_source_dir() / 'test.ref'],
    depends: [test_example_exe],
    workdir: meson.current_source_dir()
  )

  # 2. rule
  test('rule', sh,
    args: ['-c', 'echo "abcbcdabcbcdabcbcdabcbcd" | "$1" | diff -u "$2" -', 'sh', rule_example_exe.full_path(), meson.current_source_dir() / 'rule.ref'],
    depends: [rule_example_exe],
    workdir: meson.current_source_dir()
  )

  # 3. accept
  test('accept', sh,
    args: ['-c', 'echo "abcbcdabcbcdabcbcdabcbcd" | "$1" | diff -u "$2" -', 'sh', accept_example_exe.full_path(), meson.current_source_dir() / 'accept.ref'],
    depends: [accept_example_exe],
    workdir: meson.current_source_dir()
  )

  # 4. wc
  test('wc', sh,
    args: ['-c', '"$1" < "$2" | diff -u "$3" -', 'sh', wc_example_exe.full_path(), meson.current_source_dir() / 'wc.leg', meson.current_source_dir() / 'wc.ref'],
    depends: [wc_example_exe],
    workdir: meson.current_source_dir()
  )

  # 5. dc
  test('dc', sh,
    args: ['-c', 'echo "  2  *3 *(3+ 4) " | "$1" | diff -u "$2" -', 'sh', dc_example_exe.full_path(), meson.current_source_dir() / 'dc.ref'],
    depends: [dc_example_exe],
    workdir: meson.current_source_dir()
  )

  # 6. dcv
  test('dcv', sh,
    args: ['-c', 'echo "a = 6;  b = 7;  a * b" | "$1" | diff -u "$2" -', 'sh', dcv_example_exe.full_path(), meson.current_source_dir() / 'dcv.ref'],
    depends: [dcv_example_exe],
    workdir: meson.current_source_dir()
  )

  # 7. calc
  test('calc', sh,
    args: ['-c', 'echo "a = 6;  b = 7;  a * b" | "$1" | diff -u "$2" -', 'sh', calc_example_exe.full_path(), meson.current_source_dir() / 'calc.ref'],
    depends: [calc_example_exe],
    workdir: meson.current_source_dir()
  )

  # 8. basic
  test('basic', sh,
    args: ['-c', 'printf "load \\"test\\"\\nrun\\n" | "$1" | diff -u "$2" -', 'sh', basic_example_exe.full_path(), meson.current_source_dir() / 'basic.ref'],
    depends: [basic_example_exe],
    workdir: meson.current_source_dir()
  )

  # 9. localpeg
  test('localpeg', sh,
    args: ['-c', 'echo "ab.ac.ad.ae.afg.afh.afg.afh.afi.afj." | "$1" | diff -u "$2" -', 'sh', localpeg_example_exe.full_path(), meson.current_source_dir() / 'localpeg.ref'],
    depends: [localpeg_example_exe],
    workdir: meson.current_source_dir()
  )

  # 10. localleg
  test('localleg', sh,
    args: ['-c', '"$1" < "$2" | diff -u "$3" -', 'sh', localleg_example_exe.full_path(), meson.current_source_dir() / 'localleg.leg', meson.current_source_dir() / 'localleg.ref'],
    depends: [localleg_example_exe],
    workdir: meson.current_source_dir()
  )

  # 11. erract
  test('erract', sh,
    args: ['-c', 'echo "6*9" | "$1" | diff -u "$2" -', 'sh', erract_example_exe.full_path(), meson.current_source_dir() / 'erract.ref'],
    depends: [erract_example_exe],
    workdir: meson.current_source_dir()
  )
endif
