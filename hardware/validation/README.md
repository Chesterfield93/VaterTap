# Hardware validation records

One Markdown report plus raw measurement data per experiment.

Required before first event use:

- `noise-baseline.md` - rest noise over 10 minutes
- `creep-profile.md` - creep after load change, derives `settle_window_s`
- `drift-profile.md` - drift over planned event duration
- `corner-load.md` - error across all four positions
- `repeatability-200ml.md` - at least 20 reference pours, proves NFR-001
- `power-budget.md` - current draw, powerbank behaviour, runtime

Each report documents setup, wiring revision, firmware commit, instruments, procedure,
raw values, conclusion and follow-up.
