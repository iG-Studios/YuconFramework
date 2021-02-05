# Script API

`script:Preload() [REQUIRED]`

Fires when the script is first called and stored. It should be used to load any needed data.

Will yeild other scripts' preloading until the embedded preload function is complete.

---

`script:Render(Number delta) [REQUIRED] [CLIENT ONLY]`

This fires every **render frame**.
`delta` is the amount of time that passed between frames.

---

`script:Step(Number delta) [REQUIRED]`

Fires every framework tick (appx. heartbeat)
`delta` is the amount of time that passed between frames.

---

`script:Start() [REQUIRED]`

Fires when the script is initialized. Use for main coding.
