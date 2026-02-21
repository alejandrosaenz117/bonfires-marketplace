# Contributing

Thank you for contributing to Bonfires Marketplace. We welcome improvements to existing plugins and submissions of new plugins.

## How to Contribute

### Report Issues

Found a bug or missing feature in a plugin? Open a GitHub issue with:
- What the issue is
- How to reproduce it
- What you expected to happen

### Improve Existing Plugins

Each plugin has its own `references/` directory with detailed patterns and strategies. Check the plugin's README for contribution areas.

For The Devil's Advocate, improve:
- `plugins/devils-advocate/skills/devils-advocate/references/review-rubric.md` — fragility vectors, Black Swan scenarios, mitigation patterns

### Add a New Plugin

1. Create `plugins/your-plugin-name/` with structure:
   ```
   .claude-plugin/
   ├── plugin.json
   agents/
   commands/
   skills/
   README.md
   ```

2. Update `.claude-plugin/marketplace.json` with new plugin entry
3. Add your plugin's README documentation
4. Open a PR

See `plugins/devils-advocate/` for reference.

### Create a Pull Request

1. Fork the repository
2. Create a branch: `git checkout -b your-contribution-name`
3. Make your changes
4. Commit with clear message: `git commit -m "Add [description]"`
5. Push: `git push origin your-contribution-name`
6. Open a PR against `main`

### PR Guidelines

- Changes should strengthen the plugin's core capability
- Follow the plugin's existing writing style and structure
- Include concrete examples where applicable
- Test with real scenarios

---

Be respectful. Questions? Open a discussion or issue.

Thank you for strengthening Bonfires.
