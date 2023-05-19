# Helm Chart Image Updater Trigger

This composite github action is used to simplify triggering the [MortyN/helm-image-updater-action](https://github.com/MortyN/helm-image-updater-action) inside a Helm Chart repo

## Prerequisites

You will need to have the helm-image-updater-action in the target repo in order for this trigger to work

---

## Example usage in Image repo:

```yaml
on: [push]
jobs:
  helm_appversion_bumper_trigger:
    runs-on: ubuntu-latest
    name: Helm Chart Bump AppVersion - Trigger
    steps:
    
      - name: Checkout
        uses: actions/checkout@v3

      - name: Build Image #Replicating output of a build pipeline with semver
        run: IMAGEVERSION=$(echo $(( $RANDOM % 10 + 1 )).$(( $RANDOM % 10 + 1 )).$(( $RANDOM % 10 + 1 )))

      - name: Helm Chart Bump AppVersion
        uses: ./ # Uses an action in the root directory for testing
        id: triggerappversionbump
        with:
          git-repo: MortyN/helm-repo
          chart-location: charts/bettergrafana/Chart.yaml
          appversion: ${{ env.IMAGEVERSION }}
          actions-pat-key: ${{ secrets.ACTIONS_PAT_KEY }}
          
      - name: Output
        run: echo "${{ steps.triggerappversionbump.outputs.result }}"
```
