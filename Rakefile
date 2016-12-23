# coding: utf-8

require "colorize"
require "octokit"

members = %W(regrex langjun raphael-liu RichardJing hzphust xuguohan everthis crispgm haharazer)

desc "Next organizer"
task :next do
  client = Octokit::Client.new(:access_token => access_token)
  issues = client.list_issues(repo_name, options = {:state => "open"})
  member = members[0]
  index = 0
  # close issue of last week
  if !issues.empty?
    issues.each do |issue|
      next if !issue[:title].start_with? "Tech 例会 "
      index = issue[:title].split(" ").at(2)
      number = issue[:number]
      body = issue[:body]
      cur_member = body.split("<@").at(1).split(">").at(0)
      member_index = members.index(cur_member)
      if member_index >= members.length - 1
        member_index = -1
      end
      member = members[member_index+1]
      break
    end
    client.close_issue(repo_name, number)
  end
  # new issue of next week
  title = "Tech 例会 #{index+1}"
  content = <<-EOS
本周例会主持人是 <@#{member}> 请提前准备会议室并收集议题。
  EOS
  client.create_issue(repo_name, title, content)
end

def access_token
  ENV["ACCESS_TOKEN"] || ""
end

def repo_name
  "msbu-tech/meeting-schedule".freeze
end
