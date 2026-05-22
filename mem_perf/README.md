# mem_perf

`mem_perf` is an open-source memory benchmark tool inspired by `Intel mlc`.

## Build
```bash
cd mem_perf
make
```

## Supported Workloads
- Maximum bandwidth: `./mem_perf max-bw [options]`
- Latency under different levels of loads: `./mem_perf latency-sweep [options]`
- Highly dynamic bursty bandwidth: `./mem_perf dynamic-bw [options]`

Common options:

- `--cores` or `-c`: core list, e.g. `0,2,4-7` (default: all available cores)
- `--read-percent` or `-r`: read ratio `[0,100]` (default: `50`)
- `--buffer-mb` or `-b`: per-thread buffer size in MB (default: `100`)
- `--access` or `-a`: `random` or `seq` (default: `random`)

## Measuring Maximum Bandwith
```bash
./mem_perf max-bw -t 100 -r 100 -i 10 -a seq
```

`max-bw` arguments:

- `--time` or `-t`: run seconds (default: `10`)
- `--interval` or `-i`: realtime print interval in seconds (default: `1`)

Note:

- Total allocated background buffer = `buffer-mb * cores`

## Measuring under Different Levels of Loads
```bash
./mem_perf latency-sweep -S 2 -a seq -r 100 -b 64
```
`latency-sweep` arguments:

- `--sweep-seconds` or `-S`: seconds per load point (default: `2`)
- `--sweep-pcts` or `-P`: load percentages, e.g. `0,25,50,75,90,100`
- `--probe-core` or `-p`: probe core id (default: last selected core)
- `--probe-mb` or `-m`: pointer-chasing probe working set in MB (default: `256`)

## Measuring Highly Dynamic Bursty Loads
```bash
./mem_perf dynamic-bw -t 30 -i 1 -a random -r 70 -l 20 -H 100 -y 500 -d 25
```

`dynamic-bw` alternates between a low-load baseline and short high-load bursts. The active worker set rotates each burst so the pressure is not always concentrated on the same subset of cores.

Its CSV output reports interval-average load information (`avg_target_load_pct`, `avg_active_threads`) so the reported load matches the measured bandwidth window.

`dynamic-bw` arguments:

- `--time` or `-t`: run seconds (default: `10`)
- `--interval` or `-i`: realtime print interval in seconds (default: `1`)
- `--burst-period-ms` or `-y`: burst cycle period in ms (default: `1000`)
- `--burst-duty` or `-d`: percent of each cycle spent at peak load (default: `20`)
- `--base-load` or `-l`: baseline load percent (default: `20`)
- `--peak-load` or `-H`: peak burst load percent (default: `100`)
