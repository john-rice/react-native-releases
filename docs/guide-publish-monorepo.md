# Publish [Monorepo Packages](./glossary.md#monorepo-packages)

> [!Warning]
> For < 0.74, you'll need to publish any picks to monorepo packages **BEFORE** release testing. For >=0.74, you can test first but will need to complete these steps **BEFORE** final `react-native` release.

* [See instructions for < 0.74](#publishing-monorepo-packages-for-react-native-versions--074)
* [See instructions for >= 0.74](#publishing-monorepo-packages-for-react-native-versions--074-1)

## Publishing Monorepo Packages for React Native Versions < 0.74

### Step 1: Check if there are any monorepo changes

```bash
# Run in react-native repo on your release branch
yarn bump-all-updated-packages
```

If there are changes to monorepo packages, this script will ask you to confirm and commit these bumps to your local branch.

> [!Note]
> As a gut-check, the first time you run this, it should align to any pick requests/merges you made on the release branch. Just use the generic commit message.

If there are no detected changes, then there are no monorepo packages to update. You are done here and can continue with the general release.

### Step 2: Publish packages
Push the local commit to the remote release branch. This will trigger CI to publish these packages.

<figure>
<img alt="CircleCI publishing monorepo packages" src="../assets/find_and_publish_bumped_packages.png" width="400" />
<figcaption>Pushing the <code>yarn bump-all-updated-packages</code> commit to CircleCI on the 0.73 branch will trigger the <code>find_and_publish_bumped_packages</code> job which handles the npm publish.</figcaption>
</figure>

**Wait for this to complete, [see gotcha](./gotchas.md#circleci-only-runs-1-workflow-at-a-time).**

You can verify it is completed for all monorepo versions by running `print-packages` on your release branch and filtering by the minor of your release. The `--minor` flag will pull the latest version on npm registry of that minor.

This means that if your `main` (on your release branch) and the minor column match versions, then everything has been published.

<figure>
<img alt="yarn print-packages" src="../assets/yarn_print_packages.png" width="400" />
<figcaption>Running <code>yarn print-packages</code> in your release branch.</figcaption>
</figure>

### Step 3. Repeat

*Repeat steps 1-3* until `yarn bump-all-updated-packages` no longer reports any changes to monorepo packages. Make sure to wait for the publish job to complete each time.

> [!Note]
> The reason why we need to repeat steps 1-3 is because `yarn bump-all-updated-packages` script does not identify transitive dependencies on first run.

## Publishing Monorepo Packages for React Native Versions >= 0.74

No changes needed.
