
 # Check if the first line is not the "Added" section
  first_line_not_added = changelog_content.lines.first !~ /^### Added$/

  changelog_content = changelog_content.lines.drop(1).join if first_line_not_added

  # Replace the first line with the provided release name if applicable
  if first_line_not_added && !options[:release_name].to_s.empty?
    changelog_content.gsub!(/^.*$/, "## #{options[:release_name]}")
  end
