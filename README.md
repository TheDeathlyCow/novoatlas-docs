# NovoAtlas Docs

This is the documentation website for [NovoAtlas](https://www.github.com/TheDeathlyCow/novoatlas). You can find the live site at:

https://novoatlas.thedeathlycow.com/latest/

All content on this website is licensed under [CC-BY-NC-SA-4.0](./LICENSE) unless otherwise specified.

## Development

This website was built using [Zensical](https://zensical.org/) and [a zensical fork of mike](https://github.com/squidfunk/mike). It requires [Python 3](https://docs.python.org/3/using/). To setup a workspace in a virtual environment, use the following commands:

Windows setup:
```bat
python -m venv .venv  
.venv\Scripts\activate
pip install -r requirements.txt
```

Linux/Mac setup:
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Once you have a workspace, you can run a test website locally with `zensical serve`, which will deploy to http://localhost:8000. This will allow you to live refresh while you work, but it will only serve the version you checked out on Git.

## Versioning

This website provides several different versions of itself for different versions of Minecraft, as NovoAtlas has slightly different feature sets between versions. The latest version of Minecraft will always be on the `latest` branch, but if you want to work on an older branch, you should check out the branch named `release/${version}`.

To locally serve the full website with all versions, run the following commands. Note that this will not support autorefresh, so it is not that great for editing. You also dont need to use the `--push` flags as that should be handled by the github actions automatically.

```bash
# 1. if using the latest mc version
mike deploy --update-aliases "${version}" latest
# 1. if using an old mc version
mike deploy "${version}"

# 2. in either case
mike serve
```