# IQ Recorder and Scheduler for SDR#

Preview 1.0.6.0 for SDR# 1922 Beta, Windows x64. The version appears at the top of the plugin. Built with the Airspy VDK and .NET 10. Successful SpyServer captures have been reported during user testing; independent RF-quality comparison and long-duration validation remain outstanding.

## Optional compact Show/Hide panel

Check **Use compact Show/Hide panel** at the top of the full interface. The dockable plugin panel becomes a single button. Dock that launcher at the left, right, top or bottom, or stack it with other panels.

**Show** opens the full interface in a separate floating window; **Hide** removes it from view. The full window's **X** also hides it. The same interface instance is retained, including selections and edits. The choice is remembered separately for each plugin, and floating-window position and size survive a normal SDR# restart. Full windows start hidden when compact mode is restored.

Uncheck the option in the floating interface to return the full interface to its normal dockable panel. No restart is needed. If the old dock area remains large after conversion, resize it to the button height; SDR# controls and saves the surrounding docking layout. The floating full interface scrolls when resized smaller so its controls remain reachable.

Hiding a window changes presentation only. SDR# must remain open for ongoing playback, recording and scheduled jobs. Each plugin has its own launcher and floating window; neither requires the other plugin to be installed.

## Install

1. Close SDR#, including any independent recording receiver windows.
2. Extract the **preview ZIP** into the SDR# installation folder **containing SDRSharp.exe**, preserving its `Plugins` directory. The plugin DLL belongs at `Plugins\SDRSharp.IQScheduler\SDRSharp.IQScheduler.dll`. Do not extract the ZIP into `Plugins`; that creates an unintended `Plugins\Plugins` directory.
3. Start SDR# and open **Menu → Plugins → Baseband : IQ Recorder and Scheduler**.

The preview ZIP is the package for users. The source ZIP is for developers. SDR# itself, radio drivers, personal settings, test recordings, and receiver connections are not included.

Keep exactly one installed IQ Scheduler DLL under the Plugins tree. An older loose `Plugins\SDRSharp.IQScheduler.dll`, or another copy under `Plugins\Plugins`, can cause old code or duplicate plugin instances to load. With SDR# closed, move those old DLL copies to a backup folder **outside Plugins**, then extract the package at the installation root. Preserve SDRSharp.config and recordings. Verify **Plugin 1.0.6.0** appears after restarting.

## Record now

Open the **Record Now** tab. Select a receiver, center frequency, output folder, and Full Available IQ or Narrow IQ. Choose **kHz** or **MHz** beside the frequency: enter `920` with kHz selected, or `101.5` with MHz selected. Switching units converts the displayed value without changing the frequency; the selected unit is saved. Press **Record Now**; use **Stop Selected Receiver** when finished. Duration 0 means run until stopped. No schedule is required. These controls affect manual recording; they do not modify saved schedules.

Full IQ saves the source's entire input stream. Narrow IQ uses native filtering and resampling to create a smaller IQ file; it does not merely change the audio filter. Narrow bandwidth must be at least 1 kHz and no more than 80% of the input sample rate. A 16 kHz capture produces a 20 kS/s IQ file. Set the source's device sample rate and gain in SDR#'s native Source panel.

Choose sample format and file format independently. The default is **16-bit PCM IQ / WAV RF64**. Older saved plans that predate these options use that default after updating; choose **32-bit Float IQ** in each plan if you want to retain the earlier preview's float encoding. Close all receiver windows and install updates after active recordings finish.

**Sample format (bits)** and **File format** appear immediately below the frequency/capture row in both Record Now and the schedule editor. Sample format controls bit depth; sample rate follows the selected receiver, while Narrow IQ reduces the output rate according to its bandwidth setting.

## Choose a recording radio

**Current SDR# Window** uses and may retune the radio in that window. To continue DXing on another receiver, use **Add / Configure…** beside the Recording Receiver dropdown. This is available directly inside the schedule editor, so you can keep an unfinished plan open. **Recording Receivers…** on the main panel opens the same manager.

1. Choose **Add Receiver…**, enter a recognizable receiver name, and choose its radio/source from SDR#'s native list.
2. For **HF+ Series** or **R2/Mini**, select a detected USB radio by serial number. **RTL-SDR USB** lists native device names and device numbers. **Refresh** rescans without opening the radios. Device numbers can change after unplugging or reconnecting RTL hardware; Airspy profiles use serials. An Airspy already selected in this SDR# window appears as **Current SDR# Window**; choosing it uses the existing instance rather than creating another profile.
3. For **AIRSPY Server Network**, select a **Saved SpyServer**, choose **Use Full IQ**, and choose the incoming **Stream bit depth** (16-bit PCM by default, with 8-bit PCM, 24-bit PCM, and 32-bit float also available). These are receiver-stream preferences; the recording plan's file sample format is separate. Supported stream formats remain subject to the server and SDR#.
4. Choose **Add Receiver**, then **Use Selected Receiver**. The quick path saves the receiver without opening another SDR# window. Its independent window opens when a recording needs it. The plan still controls center frequency, IQ capture bandwidth, destination, and timing.

**Configure…** opens the receiver's native SDR# window for gain, hardware sample rate, connection details, or other advanced settings. **Minimize Receiver** puts that window away while reception or recording continues. For a source without quick device discovery, or if you choose **Choose in advanced SDR# setup**, the add button becomes **Add and Configure** and opens native setup. HF+/R2/RTL profiles initially reuse saved source-specific sample-rate/gain preferences; existing receiver configuration files are retained. Full IQ preference is applied when a SpyServer helper starts, and stream precision seeds its initial native configuration.

SpyServer address choices combine the main installation's `spybrowser.history`, its saved address, and the current native SpyServer panel's unsaved address/history when available. New SpyServer receivers inherit that address history. For an existing receiver, select **Import SpyServer Addresses**, then **Configure…** and open its Source-panel address dropdown. Import merges addresses and preserves the selected address and connection; it does not automatically connect. A receiver running an older plugin must be closed and reopened to support live import. No network scan or internet-wide discovery is performed.

Select **Remove Receiver…** to remove an unused saved receiver. Confirmation defaults to No. Stop its active recording and reassign/delete any schedules that reference it first, including disabled schedules. Recorded files and its receiver settings directory are kept; an existing receiver window stays open. If a recording editor still references a removed receiver, it shows Unavailable and requires explicit reselection rather than silently using the current radio.

The saved list reports Closed, Starting / awaiting response, Open · stopped, Receiving · not recording, or Recording. An open window alone does not prove that its hardware or server is connected. Closed receivers open automatically when needed. Adding a receiver does not start a recording. Names must be unique; existing profiles retain their identifiers and radio settings.

Before updating, finish recordings and close both the coordinating SDR# window and its recording receiver windows. Starting a closed receiver copies the coordinating instance's plugin build into that receiver's runtime.

The main DX window keeps its own tuning and stream. Different profiles can record simultaneously; each profile handles one recording at a time. Two profiles do not make one physical radio independently tunable. The native source still controls driver availability, device exclusivity, server access, and supported frequencies/rates.

If the intended radio is already receiving in the main SDR# window, select **Current SDR# Window** in the recording plan. Selecting a separately named profile requests a separate session, even when it uses the same hardware category. Detection lists devices; it does not guarantee that another process has released them.

Profiles keep separate SDR# copies and settings under `RecordingReceivers` in the installation folder, which must be writable. Configure and test each profile before relying on unattended work. The scheduler opens enabled jobs' receiver sessions ahead of time. Keep the coordinating SDR# instance running; closing it requests that its active recordings stop. Idle receiver windows remain open and can be closed normally.

## Schedule recordings

Open the **Schedules** tab and choose **Create Schedule…**, name the job, and select a recording plan. With no saved jobs, the tab prominently asks you to create one. Each plan has its own receiver, frequency (kHz or MHz), bandwidth and output folder. A new plan starts with the current window's tuned frequency, with a **Use This Window's Tuned Frequency** button in the editor; it does not silently inherit the Record Now controls. The list and selected-plan details show the saved frequency with its units. The editor shows the controls relevant to that plan:

- Night Watch: begin a chosen number of minutes before sunset; finish a chosen number of minutes after the following sunrise. Record around each hour, such as `:57:00` to `:07:00`, or uncheck the hourly option to record the whole night. Times recalculate for each date throughout the year. Weekday choices refer to the evening the night begins. Hourly windows that meet the night boundaries are shortened to stay inside them.
- Around Sunrise or Around Sunset: plain Start/End choices, such as **Start 60 minutes before sunrise; End 90 minutes after sunrise**. No signed offset arithmetic or duration calculation is needed.
- Specific Date, Selected Weekdays or Every Day: choose a start time and either a stop clock time or a duration. A stop time at or before the start means the following day. These overnight windows use the time-zone rules on both dates, including DST.
- Every Hour: enter a `from` and `to` minute/second. For example, `:57:00` to `:07:00` records ten minutes across the hour.
- Continuous from Date: start on the selected date/time and continue until stopped or a stop limit is reached.

Each job saves its receiver, center frequency, IQ bandwidth, destination, and limits. Solar plans accept latitude/longitude or a 4/6-character Maidenhead grid. Select the receiving site's Windows time zone explicitly; the default is the PC's zone. Coordinates do not automatically select a time zone. **Preview Next Recording** shows the next start and end in that zone and UTC, plus its duration. The main list's **Next Local** column uses the PC's local zone. Solar times are approximate, computed locally using NOAA's general solar equations; terrain and atmospheric conditions are not modeled. Dates with no required sunrise/sunset are skipped.

The mouse wheel scrolls the editor over closed dropdowns, numeric fields, and date/time fields instead of changing their values. Open a dropdown and select an item, type a value, or use arrow controls/keyboard to make an explicit edit. This prevents accidental changes while moving through a long plan.

Expand **File splitting / size limits** when needed. The editor keeps existing limits visible when a job already has them configured. Hiding these controls preserves their values.

Enabled schedules run automatically while the coordinating SDR# instance is open; there is no master scheduling switch. The list shows **Enabled** or **Disabled** separately from its **Select** checkboxes. Check one or more schedules, then choose **Enable Selected** or **Disable Selected**. **Select All** checks every schedule; it becomes **Clear Selection** when all are checked. Checking rows only selects them for an action and never changes whether they will record. The summary shows how many schedules are enabled and the next planned recording in the PC's local time. Jobs can be edited and disabled/re-enabled without deleting their settings. Disabling controls future starts; use **Stop Selected Schedule** to finish an active scheduled recording. Active edits default to the next occurrence. **Finish Current File and Apply Now** closes the current capture and starts a new one with the edited settings.

Select **Delete Schedule…** to remove an unwanted plan. A confirmation defaults to No, and recordings are kept. Stop an active scheduled recording before deleting its plan. Disabling remains the reversible option for a plan you may want again.

Upgrading from a version with a master switch preserves each schedule's own Enabled/Disabled state and ignores the old master setting. Previously enabled schedules are eligible to run even if the old master switch was off. Row-selection checkboxes are temporary and are cleared when SDR# restarts.

Receiving location and time zone are sticky: saving a plan with a location remembers them for new plans, including after restarting SDR#. On upgrade, the latest existing plan with a location seeds that default. Existing plans keep their own locations; changing the default does not alter them. Removing plans does not erase the remembered location.

Spring DST times that do not exist are skipped. Under **More time options**, choose First or Second Occurrence for the repeated autumn hour. Schedules follow the real PC clock, including when SDR# is playing a recording. The PC must remain awake; this preview does not wake it or replay missed work. Starts missed by more than five seconds are skipped. A conflicting occurrence on a busy profile is skipped rather than queued. Fixed durations run for the requested number of samples; clock/solar/hourly windows keep their planned end when a start is slightly late. Applying an active edit immediately requires the edited plan to have a recording window open now; otherwise use the default next-recording option.

## File limits and format

Job duration controls the total session. File Length and File Size control each individual file; 0 disables that limit. Choose **Split and Continue** or **Stop Recording**. If both file limits are set, the first reached wins. Container size limits cause automatic rollover into a new file. The user size limit accepts up to 1,048,576 MiB (1 TiB), subject to the selected container's own limit and available disk space.

File format choices mirror SDR# 1922's recorder:

| File format | Size behavior |
|---|---|
| WAV RF64 | 64-bit size fields support files beyond 4 GiB; the default. |
| WAV Full | Ordinary RIFF/WAVE, rotating before approximately 4 GiB. |
| WAV Strict | Ordinary RIFF/WAVE, rotating before approximately 2 GiB for compatibility. |

RF64 is different from Sony Wave64. The RF64 header follows [EBU Tech 3306-2009](https://tech.ebu.ch/files/live/sites/tech/files/shared/tech/tech3306v1_1.pdf); the Full/Strict caps match the installed SDR# 1922 recorder implementation. These are size/container choices, not compression-quality settings. All output files retain the `.wav` extension and are playable in SDR#'s Baseband File Player.

| Sample format | Bytes per complex I/Q sample | Data at 20 kS/s |
|---|---|---|
| 8-bit PCM IQ | 2 | 2.4 MB/minute |
| 16-bit PCM IQ | 4 | 4.8 MB/minute |
| 32-bit Float IQ | 8 | 9.6 MB/minute |

I is followed by Q. The 8-bit PCM encoding is unsigned; 16-bit PCM is signed. Integer encoding rounds and clips to its representable range without automatic gain normalization. Float encoding preserves the received floating-point sample values. Lower bit depths reduce available precision; 8-bit is a deliberate space/precision tradeoff. Float files cannot restore precision or bandwidth already absent from the receiver/SpyServer stream. Full Available IQ preserves the incoming sample rate; Narrow IQ intentionally filters and reduces it. Compare receiver stream settings and playback processing before attributing a listening difference to the container.

The chosen output folder is a root folder. Files are placed under `YYYY_MM_DD`, using the schedule's selected time zone or the PC's local time zone for Record Now. A continuous recording closes its file at midnight in that zone and continues into the next day's folder. This date rollover does not trigger Stop Recording unless a configured user file limit is also reached. DST days use their actual 23/24/25-hour boundaries.

Filenames contain UTC start time (`Z`), center frequency, output sample rate, and unique session/file identifiers. Each `.wav.json` sidecar includes fractional UTC start time, folder time zone, input/output rates, bandwidth, encoding and container. Timing is estimated from host buffer arrival, not a calibrated hardware/GPS timestamp. Digital filter startup/group delay is not compensated in that estimate.

The recorder reports disk/write errors, input rate changes, retunes, and buffer overruns. Normal Stop drains accepted input and closes the WAV header. A crash, power loss, or forced process termination can leave the current file unfinished. Long-running high-rate and multiple-device captures still need testing.

## Validation and preview limits

Release build: 0 warnings, 0 errors. Tests ran inside SDR# 1922 with its native DSP and Baseband File Player:

- Six-second 20 kS/s narrow recording: exactly 120,000 complex samples across four files.
- Stop-at-limit: one 1.98-second file, no restart.
- One-off scheduled full-band capture: exactly three seconds at 312,500 S/s.
- Independent receiver recording: main window tuning and playback unchanged.
- Active remote edit: old file finished; new 10 kS/s recording completed.
- Generated float IQ WAV replayed in SDR# and recaptured at 10 kS/s with finite, nonzero signal samples.
- Calendar checks for weekly/hourly recurrence, midnight, DST skipped/repeated hours, sunrise offset, grid conversion, and disabled jobs.
- Night Watch scan inside SDR#: 5,093 windows across every day of 2025, including seasonal/DST transitions and 105 clipped boundary windows; also whole-night lengths, Friday-night ownership, before/after solar ends, overnight clock stops, editor field persistence, and a native recorded-IQ capture with a fixed clock stop.
- Tabbed UI and frequency entry checked inside SDR#: 920 kHz/101.5 MHz conversion, exact Hz preservation across unit changes, saved unit preference, independent manual/scheduled settings, empty schedule guidance, and an actual scheduled capture from kHz entry with correct frequency metadata.
- All nine combinations of 8-bit/16-bit PCM/32-bit float with RF64/Full/Strict: header sizes and payload counts checked, recordings written and replayed through native SDR#. Direct writer checks verified unchanged float payloads and integer clipping/rounding. Midnight/day-folder rollover, 23/25-hour DST days, time/size splitting and Stop Recording were verified. Files larger than 4 GiB have not been stress-tested.
- Version 1.0.4.0: native SDR# checks covered individual/mixed checkbox selection, Select All/Clear Selection, bulk enable/disable, selection retention by schedule ID after rebuilding/reordering, deleted-selection cleanup, and enabled/disabled state persistence. Old master-off settings preserve each job's own state. A real HF+ IQ recording replayed through Baseband File Player produced an automatic three-second 912 kS/s RF64/16-bit capture (2,736,000 frames) with no master switch. Disabling its schedule during capture let the current file finish; a disabled job at the same start was never attempted.
- Version 1.0.3.0: receiver removal/reference/active guards, retained files, missing-receiver reselection, address-history merging (including unsaved native entries), quick-add preferences, serial seeding, and a separate native SpyServer helper honoring Full IQ and PCM 16-bit were checked inside SDR#. USB scans did not open devices; no free Airspy serials were reported while the real HF+ was in use. After releasing the main radio, the physical HF+ Discovery appeared by serial, was added without a setup window, and completed a separate scheduled three-second RF64/16-bit capture at 912 kS/s with exactly 2,736,000 frames. The busy native HF+ was also recognized as Current SDR# Window. External SpyServer streaming, physical R2/Mini and RTL-SDR selection were not verified in this pass.
- Version 1.0.2.0: receiver management from inside the schedule editor, adding/selecting a playback receiver, helper status/minimize, and preventing accidental mouse-wheel edits were checked in SDR#. Three-second scheduled and manual captures both wrote exactly 2,736,000 frames at 912 kS/s into the same correct local-date folder. Sunday deselection was not reproduced; physical receiver setup and minimizing during active helper capture were not tested in this pass.
- Version 1.0.1.0: sticky location migration/persistence, job isolation, deletion/file-retention safeguards, ID-based selection after deletion, prominent format selectors, and an active-delete refusal during an actual native 912 kS/s capture (547,200 frames completed).

A real ten-minute Night Watch capture on the HF+ Discovery also completed in its dated folder at 912 kS/s, RF64, 16-bit PCM. These checks do not establish compatibility with every physical receiver or SpyServer, unattended long-duration reliability, performance at every input rate, or RF passband/alias-rejection performance. Native source selection currently uses a guarded adapter to SDR# 1922's private source menu; a future SDR# change may require an update. The editor is functional and intentionally a first pass.

## Build from source

Use the source folder inside an Airspy VDK checkout, alongside the VDK `lib` directory. Install the .NET 10 SDK, provide SDR# as described by the VDK, then run:

```powershell
dotnet build .\IQScheduler\SDRSharp.IQScheduler.csproj -c Release -p:SDRSharpDir="C:\Path\To\SDRSharp\"
```

Close the target SDR# instance first. The project deploys into its `Plugins\SDRSharp.IQScheduler` directory. SDK reference assemblies are compile-time only; SDR# supplies the runtime libraries. No external NuGet package is required. The source package excludes the test instrumentation and receiver runtime copies.

Implementation notes: the RawIQ callback copies into a bounded native ring without file I/O or blocking. A worker performs native resampling/filtering and WAV writing. Native resources and the small per-file sidecar/name allocations occur at recording/file boundaries. Stop unregisters the stream hook before draining its final partial block, so accepted samples are preserved without recording padding.

The two tab contents receive their own theme pass, including after theme changes, because SDR#'s normal recursive theme pass does not visit TabControl pages. The containing plugin panel keeps the host's normal theme handling.

## License

See `LICENSE.txt` for the Airspy VDK license, including its Airspy ecosystem restriction. This package is intended as an SDR# plugin; the license notice must accompany redistribution.

## Live Rewind companion (1.0.5)

When Live Rewind is installed in the same SDR# window, recording uses its original live-IQ tap. Rewinding or pausing listening does not rewind the recording. Live Rewind can review the active recording, follow file splits and narrow-IQ sample rates, and retain the pre-recording buffer without writing a second ongoing recording. Earlier Scheduler versions must be updated before enabling Live Rewind.

Live Rewind holds the current IQ center while buffering/reviewing. Turn its buffer off before a scheduled job on that receiver needs to retune to a different center. Other receiver windows have independent streams. Disabling Live Rewind leaves a Scheduler recording running; its **Stop Recording & New Buffer** button explicitly finishes the local run.

The active-file index publishes sample counts after flushing buffered writes. This makes committed samples readable during recording; it is not a promise of hardware-timestamp accuracy or durable disk flush per block. Ordinary recording still works without Live Rewind installed.

Native validation of 1.0.5 in SDR# 1922 covered original live-IQ routing during rewound listening, readable committed PCM samples, four file segments, 250 kS/s narrow recording replayed at 1.2288 MS/s through the native resampler, and center-lock handoff when Live Rewind was disabled during a recording. These tests used a Linrad-derived IQ file with a test-only Live Rewind source override, rather than a physical live receiver.

## Compact launcher validation

The compact launchers were tested together in an isolated SDR# 1922 instance using the supplied Linrad-derived 1.2288 MS/s IQ WAV, DSB, binaural and a 10 kHz filter. Native checks covered Show/Hide, X-to-hide, returning to full-panel mode without restarting, preserved UI instances and selected tabs, playback and Record Now continuing while hidden, a scheduled job starting and finishing while hidden, and saved compact preferences/window geometry after restart. The existing DSP and scheduling logic were not changed. The floating detail windows use the same panels and SDR# theme service. Physical live receivers, all display scaling settings and every multi-monitor arrangement were not revalidated for this UI change.

Source builds require both the plugin folder and the accompanying **Shared** folder beside it in the VDK. Shared/PluginWindowHost.cs is compiled into each plugin, so no additional runtime DLL is needed.
