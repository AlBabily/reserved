# Define the keywords and corresponding sections
section_keywords = {
  'Added' => ['added', 'add'],
  'Changed' => ['changed', 'updated', 'upgraded', 'change', 'add', 'upgrade'],
  'Fixed' => ['fix', 'fixed'],
  'Removed' => ['remove', 'removed']
}

# Fetch commit messages between master and the release branch
commit_range = `git rev-list master..release-branch`.split("\n")

# Initialize changelog sections
changelog = {}
section_keywords.keys.each { |section| changelog[section] = [] }

# Iterate through commits and categorize based on keywords
commit_range.each do |commit_hash|
  commit_message = `git log --format=%B -n 1 #{commit_hash}`.strip.downcase

  section_keywords.each do |section, keywords|
    if keywords.any? { |keyword| commit_message.include?(keyword) }
      pr_title = commit_message.split(':').last.strip
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

