# Fork Maintenance

This HACS fork tracks WolframRavenwolf/hermes-ha-integration. Version 1.4.3
starts from upstream v1.4.2, commit
`43f4edee81f363bfc6753ef5165ffa98e770b753`.

The functional change is task-local response session metadata in `api.py`.
Home Assistant shares the client across requests. A client-wide mutable last
session ID lets overlapping conversations acquire each other's session IDs.
The concurrency regression in `tests/test_api.py` protects the boundary.

The integration keeps its upstream domain, entity identities and config
flows, so HA owns configuration and HACS owns files and updates. Install only
one repository for `hermes_conversation`; never mount a homelab source copy
over HACS's directory. Upstream retains its licence and attribution.

## Releases and Updates

1. Review upstream commits in a branch; do not automatically reset this fork
   to upstream or drop the concurrency regression.
2. Keep the fix until the test passes against upstream's replacement. Remove
   redundant local code when upstream has addressed it.
3. Run `python -m unittest discover -s tests -q` and inspect native profile,
   authentication, streaming retries and voice session changes.
4. Bump the manifest version and version-contract tests, merge after review,
   then publish the matching stable GitHub release (first: `v1.4.3`). Do not
   move an already-published release tag. Fork versions have their own release
   sequence; account for the fork's latest version when importing upstream.
5. Install the release through HACS and restart HA through the normal approved
   update procedure. Verify profile routing, two overlapping conversations,
   follow-ups and actual voice playback. For a failed update, redownload the
   previous reviewed version through HACS and restart; retain config entries.

Do not distribute Home Assistant credentials, Hermes profiles or household
data in this repository. The homelab repository manages connection policy
and runbooks, not this component's source or update lifecycle.
