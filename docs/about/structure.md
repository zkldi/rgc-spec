# Example RGC File

So you have a decent idea in your head, the below object shows
an example RGC file adhering to the ``std/iidx-SP`` schema, version 1.

!!! note
    This is a truncated file, Real charts have more than two notes!

```json
{
    "meta": {
        "schema": "std/iidx-SP",
        "rgcVersion": "0.2.0",
        "schemaVersion": 1,
        "title": "GAMBOL",
        "artist": "SLAKE",
        "charter": null
    },
    "chartMeta": {
        "difficulty": "HYPER",
        "genre": "BIG BEAT",
        "level": "2",
        "marquee": "GAMBOL"
    },
    "events": {
        "BGM": [
            {
                "ms": 0,
                "pan": 0,
                "sound": 1
            },
            {
                "ms": 0,
                "pan": 0,
                "sound": 2
            }
        ],
        "BPM": [
            {
                "ms": 0,
                "bpm": 144.95
            }
        ],
        "BSS": [],
        "HCN": [],
        "CN": [],
        "HBSS": [],
        "NOTE": [
            {
                "ms": 4144,
                "col": 6
            },
            {
                "ms": 5631,
                "col": 0
            }
        ],
        "SAMPLE": [
            {
                "ms": 0,
                "col": 0,
                "sound": 15
            },
            {
                "ms": 0,
                "col": 1,
                "sound": 2
            }
        ],
        "METER": [
            {
                "ms": 0,
                "num": 4,
                "denom": 4
            }
        ],
        "TIMINGWINDOW": [],
        "MEASUREBAR": [
            {
                "ms": 0
            },
            {
                "ms": 2451
            }
        ],
        "EOC": [
            {
                "ms": 67943
            }
        ],
        "SCRATCH": [
            {
                "ms": 3012
            },
            {
                "ms": 5731
            }
        ]
    }
}
```

In the next pages, we'll be disecting this RGC file, and explaining what each part of it does.