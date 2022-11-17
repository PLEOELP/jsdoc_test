# This is a jsdoc workflow

name: jsdoc

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the "main" branch
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - uses: actions/checkout@v3
        with:
          repository: PLEOELP/cse110-fa22-group32
          ref: jsdoc_test
      
      # Runs a single command using the runners shell
      - name: Build
        uses: andstor/jsdoc-action@v1.2.1
        with:
          source_dir: ./source/assets/scripts
          recurse: true
          output_dir: ./source/assets/doc
      
      # Commit generated document into repository
      - name: Commit
        run: |
          git config --local user.name "jsdoc_workflow"
          git add ./source/assets/doc
          git commit -m "Updating the Documentation"
      
      # Push the change
      - name: Push
        uses: ad-m/github-push-action@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          force: true
