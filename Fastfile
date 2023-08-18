
# Define the keywords and corresponding sections
section_keywords = {
  'Added' => ['added', 'add'],
  'Changed' => ['changed', 'updated', 'upgraded', 'change', 'add', 'upgrade'],
  'Fixed' => ['fix', 'fixed'],
  'Removed' => ['remove', 'removed']
}

# Fetch the diff between master and the release branch
diff_command = "git log --pretty=format:'%s' master..release-branch"
commit_messages = `#{diff_command}`.split("\n")

# Initialize changelog sections
changelog = {}
section_keywords.keys.each { |section| changelog[section] = [] }

# Iterate through commit messages and categorize based on keywords
commit_messages.each do |commit_message|
  commit_message = commit_message.downcase

  section_keywords.each do |section, keywords|
    if keywords.any? { |keyword| commit_message.include?(keyword) }
      pr_title = commit_message.split(':')[1..].join(':').strip
      changelog[section] << pr_title
      break
    end
  end
end

# Print the changelog structure
changelog.each do |section, prs|
  puts "## #{section}\n\n"
  prs.each { |pr| puts "- #{pr}\n" }
end
