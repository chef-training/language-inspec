require 'erb'
require 'highline/import'

task :default => :install

task :install do
  puts `rm -rf ~/.atom/packages/language-inspec`
  puts `cp -R ../language-inspec ~/.atom/packages/language-inspec`
end


namespace :snippets do

  resources = %w[ aide_conf apache apache_conf apt audit_policy auditd auditd_conf bash bond bridge bsd_service command cpan cran crontab csv dh_params directory docker docker_container docker_image docker_service elasticsearch etc_fstab etc_group etc_hosts etc_hosts_allow etc_hosts_deny file filesystem firewalld gem group grub_conf host http iis_app iis_site inetd_conf ini interface iptables json kernel_module kernel_parameter key_rsa launchd_service limits_conf login_defs mount mssql_session mysql_conf mysql_session nginx nginx_conf npm ntp_conf oneget oracledb_session os os_env package packages parse_config parse_config_file passwd pip port postgres_conf postgres_hba_conf postgres_ident_conf postgres_session powershell processes rabbitmq_config registry_key runit_service security_policy service shadow ssh_config sshd_config ssl sys_info systemd_service sysv_service upstart_service user users vbscript virtualization windows_feature windows_hotfix windows_task wmi x509_certificate xinetd_conf xml yaml yum zfs_dataset zfs_pool azure_generic_resource azure_resource_group azure_virtual_machine azure_virtual_machine_data_disk aws_cloudtrail_trail aws_cloudtrail_trails aws_cloudwatch_alarm aws_cloudwatch_log_metric_filter aws_config_recorder aws_ec2_instance aws_iam_access_key aws_iam_access_keys aws_iam_group aws_iam_groups aws_iam_password_policy aws_iam_policies aws_iam_policy aws_iam_role aws_iam_root_user aws_iam_user aws_iam_users aws_kms_keys aws_route_table aws_s3_bucket aws_security_group aws_security_groups aws_sns_topic aws_subnet aws_subnets aws_vpc aws_vpcs ]

  namespace :resources do
    desc 'adds / removes / and updates snippets'
    task :update do

    end

    desc 'purge resource snippet file that are no longer in the resources list'
    task :purge do
      # Ignore any snippets that start with an underscore
      resource_snippets = Dir['snippets/*.cson'].reject { |snippet_path| File.basename(snippet_path).start_with?('_') }

      resource_snippets.each do |snippet_path|
        snippet_name = File.basename(snippet_path).gsub('.cson','')
        unless resources.include?(snippet_name)
          puts "Removing the snippet #{snippet_name} at #{snippet_path}"
          `git rm #{snippet_path}`
        end
      end
    end

    desc 'generate resource snippets'
    task :generate do
      template = ERB.new File.read('snippet-template.cson')

      existing_resources = resources.find_all { |resource| File.exist?("snippets/#{resource}.cson") }
      new_resources = resources - existing_resources

      new_resources.each do |resource|
        @resource = resource
        File.write("snippets/#{resource}.cson",template.result(binding))
      end
    end

    desc "Helps with creating commits for each snippets. Asks for the snippet name"
    task :commit do
      name = ask "resource to commit?"
      puts `git add snippets/#{name}.cson`
      puts `git commit -m "Updating #{name} resource snippet"`
    end

    task :remove do
      resources.each do |resource|
        # `rm snippets/#{resource}.cson`
      end
    end

    desc "Exports all the CSON snippest to JSON into the VSCode Extension"
    task :export do
      # resources.each do |resource|
      #   puts "Exporting #{resource}"
      #   `cson2json snippets/#{resource}.cson > ../extension-inspec/snippets/#{resource}.json`
      # end

      json_result = resources.map do |resource|
        "{\n    \"language\": \"inspec\",\n    \"path\": \"./snippets/#{resource}.json\"\n}"
      end

      puts json_result.join(",\n")
    end
  end

end
