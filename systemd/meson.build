services = [
  'sway.service',
  'wayland-session-pre.target',
  'wayland-session.target',
]

in_services = [
  'swayidle.service',
]

conf = configuration_data({
  'prefix': get_option('prefix')
})
foreach srv : in_services
  configure_file(
    input: srv + '.in',
    output: srv,
    configuration: conf,
    install_dir: 'lib/systemd/user',
    install_mode: 'rw-r--r--',
  )
endforeach

install_data(services, install_dir: 'lib/systemd/user')
