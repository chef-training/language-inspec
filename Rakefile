require 'erb'

task :default => :install

task :install do
  puts `rm -rf ~/.atom/packages/language-inspec`
  puts `cp -R ../language-inspec ~/.atom/packages/language-inspec`
end


namespace :snippets do

  resources = %w[ apache_conf apt audit_policy auditd_conf auditd_rules bash bond bridge bsd_service command csv directory etc_group etc_password etc_shadow file gem group grub_conf host iis_site inetd_conf ini interface iptables json kernel_module kernel_parameter launchd_service limits_conf login_def mount mysql_conf mysql_session npm ntp_conf oneget os os_env package parse_config parse_config_file pip port postgres_conf postgres_session powershell process registry_key runit_service security_policy service ssh_conf sshd_config ssl sys_info systemd_service sysv_service upstart_service user users vbscript windows_feature wmi xinetd_conf yaml yum ]

  namespace :resources do
    desc 'generate resource snippets'
    task :generate do
      template = ERB.new File.read('snippet-template.cson')

      resources.each do |resource|
        @resource = resource
        # File.write("snippets/#{resource}.cson",template.result(binding))
      end
    end

    task :remove do
      resources.each do |resource|
        # `rm snippets/#{resource}.cson`
      end
    end
  end

end
