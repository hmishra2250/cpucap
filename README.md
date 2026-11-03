# cpucap

Tame a hot Intel laptop from the terminal. One small bash script that caps CPU speed and turns Turbo Boost on or off through the `intel_pstate` driver.

I wrote this for an i7-14700HX laptop. It sat at 95–97°C while nearly idle and throttled itself down to about 700 MHz. Turning turbo off and capping the maximum performance keeps it cool, and it ends up faster than when it was throttling.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/hmishra2250/cpucap/main/cpucap -o ~/bin/cpucap
chmod +x ~/bin/cpucap
```

## Usage

```bash
sudo cpucap cool     # turbo off, max ~60%  — everyday work, big heat drop
sudo cpucap quiet    # turbo off, max ~45%  — coolest / quietest
sudo cpucap normal   # turbo off, no cap    — builds without turbo spikes
sudo cpucap full     # stock: turbo on, 100%
cpucap status        # show mode, temperature, avg clock (no sudo needed)
```

The percentages are of the CPU's maximum turbo frequency, so on a 5.3 GHz chip `cool` is about 3.2 GHz.

## Requirements

- Linux with the `intel_pstate` driver. Check with `cat /sys/devices/system/cpu/cpufreq/policy0/scaling_driver`.
- Root access to change modes.

## Notes

- Settings reset at reboot. To make one stick, run it from a systemd unit or root's crontab (`@reboot /path/to/cpucap cool`).
- If your laptop runs hot while idle, also check the vents for dust and use your vendor's fan boost (for example MSI Cooler Boost).

## License

MIT
