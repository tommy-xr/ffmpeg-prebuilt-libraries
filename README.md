# ffmpeg-prebuilt-libraries

## Cutting a release

1. Push any source/pipeline updates you want included in the binaries.
2. Open the **Actions** tab and run the **Release FFmpeg binaries** workflow.
3. Fill in the required inputs:
   - `release_tag`: git tag that should back the release (for example `v0.6.0`).
   - `release_name` / `release_body`: optional metadata shown on the GitHub release page.
   - `ffmpeg_version`: upstream FFmpeg tag to build (defaults to `n7.0`).
   - `draft` / `prerelease`: toggle release visibility.
4. Wait for all build jobs (Linux, macOS Intel, macOS Apple Silicon, Android arm64-v8a, Windows x64) to finish. Each job packages the content that lands in `ffmpeg/out*` into a `<platform>.tar.gz`.
5. The `Publish release` job creates/updates the GitHub release for the provided tag and attaches all generated archives so the binaries live with the release itself.

If the workflow is rerun for the same tag, the publish step removes any previous assets with the same filenames before uploading the fresh builds. This guarantees that the Android arm64 shared libs plus the Windows DLLs on the release are always in sync with the latest pipeline run.

## Per-platform builds

Each `Build FFmpeg (…)` workflow in `.github/workflows` now exposes the same `ffmpeg_version` input via both `workflow_dispatch` and `workflow_call`. That means you can manually trigger an individual platform build for ad-hoc verification, or reuse the workflow from another workflow (like the release pipeline) without duplicating the setup/build/packaging logic.
