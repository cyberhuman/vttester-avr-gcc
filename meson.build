project('ttester', 'c')

# Get VREF configuration from meson options
vref_voltage = get_option('vref_voltage').to_string()
vref_scale = get_option('vref_scale').to_string()

add_global_arguments([
  '-Werror',
  ],
  language : [ 'c', 'cpp' ])

exe = executable('ttester', 'TTesterLCD.c', c_args: [
  '-DVREF_VOLTAGE=' + vref_voltage,
  '-DVREF_SCALE=' + vref_scale,
])

avr_objcopy = find_program('avr-objcopy')
hex = custom_target(
  'firmware HEX',
  input : exe,
  output : 'firmware.hex',
  command : [ avr_objcopy, '-j', '.text', '-j', '.data', '-O', 'ihex', '@INPUT@', '@OUTPUT@' ],
  build_by_default : true)
eep = custom_target(
  'firmware EEP',
  input : exe,
  output : 'firmware.eep',
  command : [
    avr_objcopy,
    '--set-start', '0',
    '--change-section-lma', '.eeprom=0',
    '--set-section-flags', '.eeprom=alloc,load',
    '-j', '.eeprom',
    '-O', 'ihex',
    '@INPUT@', '@OUTPUT@'
    ],
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
