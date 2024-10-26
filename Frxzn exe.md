name: Generate Key

on:
  workflow_dispatch:

jobs:
  generate-key:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Generate Unique Key File
        env:
          PLAYER_ID: ${{ github.event.inputs.player_id }}
          HWID_KEY: ${{ github.event.inputs.hwid_key }}
        run: |
          echo "# Player Key for ${PLAYER_ID}" > keys/${PLAYER_ID}.md
          echo "Your key is: ${HWID_KEY}" >> keys/${PLAYER_ID}.md
          echo "This key will expire in 24 hours." >> keys/${PLAYER_ID}.md

      - name: Commit and Push Key File
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git add keys/${PLAYER_ID}.md
          git commit -m "Generate key for ${PLAYER_ID}"
          git push
