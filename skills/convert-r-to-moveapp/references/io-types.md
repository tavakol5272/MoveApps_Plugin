# MoveApps IO Types — Reference
Apps can only chain together in a Workflow when one App's output type
matches the next App's input type. Once an App is initialized on
MoveApps, its IO types are permanently fixed.

- **`move2::move2_loc`** — location/tracking data only. Replaces the
  deprecated `moveStack`. A `move2` object can technically hold both
  location and non-location data, but this IO type is restricted to the
  location-only case by design (location and non-location data are kept
  analytically separate on the platform). This is the default assumption
  for this skill.

- **`move2::move2_nonloc`** — non-location data only (e.g. acceleration
  or other accessory sensor measurements tied to a track). Same `move2`
  object class as above, but restricted to the non-location case.
  Combining loc and nonloc data in a single App is not currently
  supported by the platform.

- **`ctmm::telemetry.list`** — a list of `ctmm` telemetry objects (one
  per track/animal), for the R `ctmm` package's continuous-time movement
  modeling functions. **Out of scope for this skill** — stop and flag if
  code data matches this shape (see step 3 of `SKILL.md`).

- **`ctmm model with data`** — a length-2 list: fitted `ctmm` models,
  plus the corresponding `ctmm` telemetry data. **Out of scope.**

- **`ctmm ud with data`** — a length-3 list: fitted `ctmm` models,
  fitted utilization distributions (UDs), and the telemetry data.
  **Out of scope.**

- **`move::moveStack`** — deprecated. Never use for new Apps; if
  original code uses this, migrate to `move2_loc` instead of preserving
  it.

  ## If none of these types fit

If the code's data doesn't match any of the types above, this skill
cannot determine a valid IO type — stop and tell the user. 
It is possible to request new IO types that will then become available 
for use in Apps that can be submitted and integrated into the platform. 
MoveApps supports requesting an entirely new IO type via
moveapps.org/apps/io-type/request.
Point the user to this option rather than forcing a mismatched type.

## Translators ??????
If the data is location-based, mention that Translator Apps exist on the platform 
to bridge between equivalent location-type IO types
