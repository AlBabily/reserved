lane :release do |options|
  release_name = options[:release_name] || ''
  start_commit_hash = options[:start_commit_hash]
  today_date = Time.now.strftime('%Y-%m-%d')
  changelog_file_path = '../CHANGELOG.md'

  head_commit_hash = `git rev-parse HEAD`.strip
  changelog_content = File.read(changelog_file_path)
  release_changelog = changelog_content.dup

  if !release_name.empty?
    release_changelog.gsub!(/^### .*$/, "### #{release_name}\n\n")
  end

  release_changelog.gsub!(/^## Added.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Added.*(?=\n## )/)
  release_changelog.gsub!(/^## Changed.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Changed.*(?=\n## )/)
  release_changelog.gsub!(/^## Removed.*(?=\n## )/, "\\0\n") if release_changelog.match?(/^## Removed.*(?=\n## )/)
  release_changelog.gsub!(/^## Fixed.*(?=\n## )/, "\\0\n\n") if release_changelog.match?(/^## Fixed.*(?=\n## )/)

  build_info = "[Version #{options[:version_code]}_#{options[:build_number]}] - Date #{today_date}"
  hash_info = "### Hash #{head_commit_hash}"
  release_changelog += "\n\n#{build_info}\n\n#{hash_info}"

  File.write(changelog_file_path, release_changelog)
  UI.success("Release changelog generated at #{changelog_file_path}")
end

