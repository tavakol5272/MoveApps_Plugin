# [Publish and distribute a plugin:](https://code.claude.com/docs/en/plugins/publish)


[Choose how to distribute:](https://code.claude.com/docs/en/plugins/publish#choose-how-to-distribute):

Your own marketplace:
1- Who can install: Anyone who can reach the repository, which can be a private one your team can clone
2- What you need: A git repository or other host with a .claude-plugin/marketplace.json that lists your plugin


### Publish through your own marketplace
Your own marketplace is a .claude-plugin/marketplace.json file that lists your plugin, added to a git repository. Once the file is in the repository, the plugin is published, with no submission form. You can keep the file in the plugin’s own repository or in a separate one.

#### Add the marketplace file to your repository
To publish from the plugin’s own repository, save the marketplace file beside plugin.json in .claude-plugin/, with one entry whose source is "./", the repository root. Give the entry the same name as plugin.json, per Keep the entry name and the manifest name the same.

#### Control who can install
Anyone who can clone the repository can install from it, so if the repository is private, the marketplace is private too. For hosts other than a git repository, see [Host a marketplace](https://code.claude.com/docs/en/plugins/host-marketplace). To reach everyone at a company, including people who don’t use git, see Roll out to a whole company.



























[Host and maintain a marketplace](https://code.claude.com/docs/en/plugins/host-marketplace)
