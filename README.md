# cpucap

Tame a hot Intel laptop from the terminal. One small bash script that caps CPU speed, turns Turbo Boost on or off, and limits package power. It uses the `intel_pstate` driver and Intel RAPL.

I wrote this for an i7-14700HX laptop. It sat at 95–97°C while nearly idle and throttled itself down to about 700 MHz. The firmware let the CPU draw 200 W, far more than the laptop could cool. Turning turbo off, capping the clock and limiting power to 30–45 W keeps it cool, and it ends up faster than when it was throttling.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/hmishra2250/cpucap/main/cpucap -o ~/bin/cpucap
chmod +x ~/bin/cpucap
```

## Usage

```bash
sudo cpucap quiet    # coolest / quietest
sudo cpucap cool     # everyday work, big heat drop
sudo cpucap normal   # builds without turbo spikes
sudo cpucap full     # stock: turbo on, firmware power limits
cpucap status        # show mode, power limit, temperature, avg clock (no sudo needed)
```

| Mode | Turbo | Max performance | Power limit (sustained / burst) |
|---|---|---|---|
| `quiet` | off | 45% (~2.4 GHz) | 30 W / 40 W |
| `cool` | off | 60% (~3.2 GHz) | 45 W / 65 W |
| `normal` | off | 100% | 65 W / 90 W |
| `full` | on | 100% | firmware defaults |

The percentages are of the CPU's maximum turbo frequency, so on a 5.3 GHz chip `cool` is about 3.2 GHz. The power limit sets how much heat the CPU can produce at all, so it does the most to lower temperature. Before its first change, `cpucap` saves the firmware's power limits in `/run`, and `full` restores them from there.

## Requirements

- Linux with the `intel_pstate` driver. Check with `cat /sys/devices/system/cpu/cpufreq/policy0/scaling_driver`.
- For power limits, Intel RAPL at `/sys/class/powercap/intel-rapl:0`. If it's missing, `cpucap` skips the power limit and still caps turbo and clock speed.
- Root access to change modes.

## Notes

- Settings, including the power limits, reset at reboot. To make one stick, run it from a systemd unit or root's crontab (`@reboot /path/to/cpucap cool`).
- If your laptop runs hot while idle, also check the vents for dust and use your vendor's fan boost (for example MSI Cooler Boost).

## License

MIT
