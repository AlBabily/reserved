
lane :release do |options|
  # Retrieve parameters from options
  release_name = options[:release_name] || ''  # Use provided release name or empty string
  start_commit_hash = options[:start_commit_hash]  # The provided commit hash
  today_date = Time.now.strftime('%Y-%m-%d')

  # Define the path to your changelog file
  changelog_file_path = '../CHANGELOG.md'  # Specify your desired path

  # Retrieve the last commit hash
  head_commit_hash = `git rev-parse HEAD`.strip

  # Read the existing changelog content
  changelog_content = File.read(changelog_file_path)

  # Update the release changelog content
  release_changelog = changelog_content.dup

  # Replace the release name if provided
  if !release_name.empty?
    release_changelog.gsub!(/^### .*$/, "### #{release_name}\n\n")
  end

  # Add newlines if necessary
  release_changelog.gsub!(/^## Added.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Added.*(?=\n## )/)
  release_changelog.gsub!(/^## Changed.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Changed.*(?=\n## )/)
  release_changelog.gsub!(/^## Removed.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Removed.*(?=\n## )/)
  release_changelog.gsub!(/^## Fixed.*(?=\n## )/, "\\0\n\n") if release_changelog.match?(/^## Fixed.*(?=\n## )/)

  # Add build info and hash at the bottom
  build_info = "[Version #{options[:version_code]}_#{options[:build_number]}] - Date #{today_date}"
  hash_info = "### Hash #{head_commit_hash}"
  release_changelog += "\n\n#{build_info}\n\n#{hash_info}"

  # Write the release changelog content back to the file
  File.write(changelog_file_path, release_changelog)

  UI.success("Release changelog generated at #{changelog_file_path}")
end
