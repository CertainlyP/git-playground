# Threat Hunting Blog - User Guide

## Quick Start

Your GitHub Pages blog is now set up! Follow these steps to publish it:

### 1. Enable GitHub Pages

1. Go to your repository on GitHub: `https://github.com/CertainlyP/git-playground`
2. Click **Settings** → **Pages** (in the left sidebar)
3. Under "Source", select the branch `claude/create-github-blog-011CUPYuJcFUqWguFfRZ7BPs` (or `main` after merging)
4. Click **Save**
5. Your site will be published at: `https://certainlyp.github.io/git-playground/`

### 2. Writing Blog Posts

Create new posts in the `_posts/` directory with this naming format:

```
YYYY-MM-DD-title-of-post.md
```

**Example:** `2025-10-23-hunting-suspicious-powershell-execution.md`

#### Post Template

```markdown
---
layout: post
title: "Your Post Title"
date: 2025-10-23
categories: threat-hunting detection
tags: [windows, malware, sigma, kql]
---

## Introduction

Your content here...

### Code Examples

```kusto
// Your detection queries
DeviceProcessEvents
| where FileName =~ "powershell.exe"
```

## Analysis

Your analysis...

## Conclusion

Key takeaways...
```

### 3. Post Structure Best Practices

For threat hunting content, include:

- **Overview** - What threat/technique you're analyzing
- **The Threat** - Description of the adversary technique
- **Hunting Techniques** - Detection queries (KQL, SPL, Sigma)
- **Analysis Findings** - What you discovered
- **MITRE ATT&CK Mapping** - Relevant tactics and techniques
- **Detection Recommendations** - How to detect this in production
- **IoCs** - Indicators of compromise (if applicable)
- **References** - Links to additional resources

### 4. Supported Query Languages

The blog has syntax highlighting for:

- **KQL** (Kusto Query Language) - Microsoft Sentinel, Defender
- **SPL** (Splunk Processing Language) - Splunk
- **YAML** - Sigma rules
- **PowerShell** - Scripts and commands
- **Python** - Analysis scripts
- **Bash** - Shell commands
- **SQL** - Database queries

### 5. Using Tables for IoCs

```markdown
| Indicator | Type | Verdict |
|-----------|------|---------|
| evil.exe | File | Malicious |
| 192.0.2.1 | IP | Suspicious |
```

### 6. Adding Images

Place images in `assets/images/` and reference them:

```markdown
![Screenshot]({{ site.baseurl }}/assets/images/screenshot.png)
```

### 7. Local Testing (Optional)

To test your blog locally before publishing:

```bash
# Install Jekyll
gem install bundler jekyll

# Create Gemfile
cat > Gemfile << EOF
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
gem "webrick"
EOF

# Install dependencies
bundle install

# Run local server
bundle exec jekyll serve

# Visit http://localhost:4000
```

## Blog Workflow

### Publishing New Content

```bash
# 1. Create your post
nano _posts/2025-10-23-my-new-post.md

# 2. Add to git
git add _posts/2025-10-23-my-new-post.md

# 3. Commit
git commit -m "Add new post: My New Post"

# 4. Push to GitHub
git push -u origin your-branch-name

# GitHub Pages will automatically build and publish!
```

## Customization

### Update Blog Info

Edit `_config.yml` to customize:

```yaml
title: Your Blog Title
description: Your description
author: Your Name
email: your-email@example.com
```

### Change Theme

GitHub Pages supports several themes. Update `_config.yml`:

```yaml
theme: minima  # or: jekyll-theme-hacker, jekyll-theme-slate, etc.
```

Available themes:
- `minima` (clean, professional)
- `jekyll-theme-hacker` (terminal style - great for security!)
- `jekyll-theme-slate`
- `jekyll-theme-cayman`

### Custom Styling

Add custom CSS in `assets/css/custom.css` - already set up with:
- Code block styling
- Table formatting for IoC tables
- Alert boxes for important findings
- Tag styling

## Content Ideas for Threat Hunting

- **Detection Engineering** - New Sigma/YARA rules
- **Hunt Reports** - Results from proactive hunts
- **Technique Analysis** - Deep dives into specific TTPs
- **Tool Reviews** - Security tools and platforms
- **Case Studies** - Real incident analysis (sanitized)
- **Query Libraries** - Useful detection queries
- **Automation Scripts** - Python/PowerShell automation
- **Threat Intel Analysis** - Analysis of new threats

## Security Considerations

### Safe Defanging

Always defang malicious indicators:

```
Good:
- hxxp://evil[.]com
- 192.0.2[.]1
- evil[.]exe

Bad:
- http://evil.com  ❌ Don't include live malicious URLs!
```

### Sample Data

Use RFC 5737 addresses for examples:
- `192.0.2.0/24`
- `198.51.100.0/24`
- `203.0.113.0/24`

### Responsible Disclosure

- Don't publish 0-days without coordinating disclosure
- Sanitize sensitive data from logs/screenshots
- Use placeholder names for organizations

## Resources

- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Markdown Guide](https://www.markdownguide.org/)
- [Sigma Rule Specification](https://github.com/SigmaHQ/sigma-specification)

## Example Posts to Write

1. **"Hunting for Living-off-the-Land Binaries (LOLBins)"**
2. **"Building a Sigma Rule for [New Threat]"**
3. **"Anomaly Detection in Windows Event Logs"**
4. **"Threat Hunting with KQL: Advanced Techniques"**
5. **"Analyzing Cobalt Strike Beacons"**

---

Happy blogging and happy hunting! 🎯
