# RGC Hashing

RGC describes two methods to hash charts, this ensures that people
can cross-identify RGC files.

## Sync-Agnostic (Default)

This is the default way to identify RGC files.

This algorithm neutralises constant-offsets in MS, which means that say, file X and file X with all events +15ms produce the same hash.

This fixes an issue which is related to another issue in games like StepMania,
where there is no way to constantly offset a file, and people insist that +14ms
is the right way to play charts on a cabinet. This results in everyone
essentially playing "different" charts.

The algorithm is implemented as follows.

```js
// StableStringify wrote by zkldi
// can be acquired under fast-json-stable-hash
// licensed under MIT, but you cant really license iterating over an object.
function StableStringify(obj) {
    const type = typeof obj;

    if (type === "string") {
        return JSON.stringify(obj);
    } else if (Array.isArray(obj)) {
        let str = "[";

        let al = obj.length - 1;

        for (let i = 0; i < obj.length; i++) {
            str += StableStringify(obj[i]);

            if (i !== al) {
                str += ",";
            }
        }

        return `${str}]`;
    } else if (type === "object" && obj !== null) {
        let str = "{";
        let keys = Object.keys(obj).sort();

        let kl = keys.length - 1;

        for (let i = 0; i < keys.length; i++) {
            let key = keys[i];
            str += `${JSON.stringify(key)}:${StableStringify(obj[key])}`;

            if (i !== kl) {
                str += ",";
            }
        }

        return `${str}}`;
    } else if (type === "number" || type === "boolean" || obj === null) {
        // bool, num, null have correct auto-coercions
        return `${obj}`;
    } else {
        throw new TypeError(
            `Invalid JSON type of ${type}, value ${obj}. FJSS can only hash JSON objects.`
        );
    }
}

// Calculates the Sync-Agnostic Hash for an RGC file.
// **this requires that RGC is sorted according to the Consistent Event Sorting Algorithm.**
function rgcSAH(rgc) {
    let firstEventMS = null;

    // find the first event in this chart
    for (const type in rgc.events) {
        let ev = rgc.events[type][0];

        if (!ev) {
            continue;
        }

        if (firstEventMS === null) {
            firstEventMS = ev.ms;
        }
        else if (ev.ms < firstEventMS) {
            firstEventMS = ev.ms;
        }
    }

    if (firstEventMS === null) {
        // chart has no events? set to 0 anyway.
        firstEventMS = 0;
    }

    // you can rewrite this into a streaming alg if you need to squeeze
    // performance
    let data = "";

    // Make chart sync-agnostic by normalising to first event.
    for (const type in rgc.events) {
        for (const event of rgc.events[type]) {
            event.ms -= firstEventMS;
        }
    }

    // omit the meta field
    let objToHash = {
        chartMeta: rgc.chartMeta,
        events: rgc.events
    }

    // acquire sha256 from where-ever.
    return HashSha256(StableStringify(objToHash));
}
```

## Sync-Dependent

A more naive way to identify charts is the sync-dependent hash.

An implementation reference can be found below:

```js
// StableStringify taken from above.

function rgcSDH(rgc) {
    // omit the meta field
    let objToHash = {
        chartMeta: rgc.chartMeta,
        events: rgc.events
    }

    // acquire sha256 from where-ever.
    return HashSha256(StableStringify(objToHash));
}

```

## Testing Implementation

Should you have to port these implementations, test data can be found at (TODO). 

