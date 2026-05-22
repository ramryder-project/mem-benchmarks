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
