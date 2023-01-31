# Carbon Simulator (JupyterLite Version)
_v0.9 - 31/Jan/2023_

- **[the Demo7-4 notebook on the deployed JupyterLite instance][url_nb]**
 (notebook [on Binder][binder_latest74])
- the associated [github repo][gh] ([actions status][gh_actions]; [actions setting][gh_action_set]; [pages settings][gh_pages_set])
- the `carbon simulator` [on github][simrepo], [pypi][simpypi] and various notebooks [on Binder][binderrepo]
- the Carbon [website][carbonxyz], [whitepaper][whitepaper] and [litepaper][litepaper]

[url]:https://sklbancor.github.io/carbon-sim-jupylite/lab/index.html
[url_nb]:https://sklbancor.github.io/carbon-sim-jupylite/lab?path=demo7-4%2Fdemo7-4.ipynb
[gh]:https://github.com/sklbancor/carbon-sim-jupylite
[gh_actions]:https://github.com/sklbancor/carbon-sim-jupylite/actions
[gh_action_set]:https://github.com/sklbancor/carbon-sim-jupylite/settings/actions
[gh_pages_set]:https://github.com/sklbancor/carbon-sim-jupylite/settings/pages

[carbonxyz]:https://carbondefi.xyz
[whitepaper]:https://carbondefi.xyz/whitepaper
[litepaper]:https://carbondefi.xyz/litepaper
[simrepo]:https://github.com/bancorprotocol/carbon-simulator/
[simpypi]:https://pypi.org/project/carbon-simulator/
[binderrepo]:https://github.com/bancorprotocol/carbon-simulator-binder/
[binder_latest74]:https://mybinder.org/v2/gh/bancorprotocol/carbon-simulator-binder/latest_7_4?labpath=Frozen%2FDemo7-4%2FDemo7-4.ipynb

---

Doc on Jupyterlite is [here][jupyterlite], repo  is [here][jupyterliter], and some getting-started is [here][codesolid]. Repo template is [here][template].


[codesolid]:https://codesolid.com/jupyter-lite-python-in-the-browser-with-serverless-jupyter/
[jupyterlite]:https://jupyterlite.readthedocs.io/en/latest/quickstart/deploy.html
[template]:https://github.com/jupyterlite/demo
[jupyterliter]:https://github.com/jupyterlite/jupyterlite

---

Required DNS settings

    Record Type: CNAME
    Record Name: jupytersim01 (.bancordefi.xyz)
    Value: sklbancor.github.io

---

Original `requirements.txt`

    bqplot
    ipycanvas>=0.9.1
    ipyevents>=2.0.1
    ipyleaflet
    ipympl>=0.8.2
    ipywidgets>=8.0.0,<9
    jupyterlab~=3.5.1
    jupyterlab-drawio
    jupyterlab-language-pack-fr-FR
    jupyterlab-language-pack-zh-CN
    jupyterlab-fasta>=3,<4
    jupyterlab-geojson>=3,<4
    jupyterlab-tour
    jupyterlab_miami_nights
    jupyterlite==0.1.0b18
    jupyterlite-p5-kernel==0.1.0
    jupyterlite-xeus-lua==0.3.1
    jupyterlite-xeus-wren==0.2.1
    jupyterlite-xeus-sqlite==0.2.1
    plotly>=5,<6
    theme-darcula

---

Original `deploy.yml`

    name: Build and Deploy

    on:
    push:
        branches:
        - main
    pull_request:
        branches:
        - '*'

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
            uses: actions/checkout@v3
        - name: Setup Python
            uses: actions/setup-python@v4
            with:
            python-version: '3.10'
        - name: Install the dependencies
            run: |
            python -m pip install -r requirements.txt
        - name: Build the JupyterLite site
            run: |
            cp README.md content
            jupyter lite build --contents content --output-dir dist
        - name: Upload artifact
            uses: actions/upload-pages-artifact@v1
            with:
            path: ./dist

    deploy:
        needs: build
        if: github.ref == 'refs/heads/main'
        permissions:
        pages: write
        id-token: write

        environment:
        name: github-pages
        url: ${{ steps.deployment.outputs.page_url }}

        runs-on: ubuntu-latest
        steps:
        - name: Deploy to GitHub Pages
            id: deployment
            uses: actions/deploy-pages@v1



