project('ttester', 'c')

add_global_arguments([
  '-Werror',
  ],
  language : [ 'c', 'cpp' ])

exe = executable('ttester', 'TTesterLCD.c')

avr_objcopy = find_program('avr-objcopy')
hex = custom_target(
  'firmware HEX',
  input : exe,
  output : 'firmware.hex',
  command : [ avr_objcopy, '-j', '.text', '-j', '.data', '-O', 'ihex', '@INPUT@', '@OUTPUT@' ],
  build_by_default : true)

avrdude = find_program('avrdude')

flash_op_legacy = ':'.join([ 'flash', 'w', hex.full_path() ])
flash_legacy = run_target(
  'flash_legacy',
  command : [ avrdude, '-p', 'm32a', '-c', 'avrisp', '-P', '/dev/ttyUSB0', '-b', '115200', '-U', flash_op_legacy ],
  depends : hex)

flash_op = ':'.join([ 'flash', 'w', exe.full_path() ])
flash = run_target(
  'flash',
  command : [ avrdude, '-p', 'm32a', '-c', 'avrisp', '-P', '/dev/ttyUSB0', '-b', '115200', '-U', flash_op ],
  depends : exe)
