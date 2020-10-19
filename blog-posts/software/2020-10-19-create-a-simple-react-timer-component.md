I have recently been developing a workout application during my spare time. One of the requirements I set for it was to create a timer so that users could keep track of their workouts. The primary goal was to build a timer which featured a way to "play", "pause", and "stop" a workout. Additionally, it would need to store enough information so that questions such as "How much time did the user take to complete the exercise?" or "How much time did it take to complete the entire workout?" could be answered.

In this blog post, I will explain a simple implementation of a timer component in React that can be extended to answer the aforementioned questions. At the end, there are links to a CodeSandbox demo and the GitHub repository of the code.

## The Plan 💡

The main idea was to create an entity which would allow to store all the information that is needed. This entity would store when it started,  paused, and for how much time it ran. Let's call this entity a "time entry" and define it as follows:

```jsx
{
  startedAt: Integer, // The # of elapsed ms since the unix epoch
  elapsedMs: Integer // If paused, the # of ms this time entry ran
}
```

A workout would then be defined as a list of time entries. In other words, each time the user started the timer, it would initialize a time entry and set `startedAt` to "now". It would keep running unless paused, in which case the number of elapsed milliseconds since it was started would be computed and stored in `elaspedMs`. If the timer is started again, then a new time entry would be created. Finally, computing the total elapsed time would simply require adding up all the time entries' `elapsedMs` .

## The Timer Reducer ⚒️

Let's go ahead and implement it using [CRA](https://github.com/facebook/create-react-app) to simplify the process. Run `npx create-react-app react-timer-app` to create the application.

I'll be using the "[State Reducer Pattern](https://kentcdodds.com/blog/the-state-reducer-pattern-with-react-hooks)" as explained by Kent C. Dodds. Let's start by defining a simple skeleton of the timer reducer, the actions the user will be allowed to perform, and the `useTimer` hook in `App.js` as follows:

```jsx
const actionTypes = {
  tick: 'tick',
  play: 'play',
  pause: 'pause',
  stop: 'stop',
}

const initialState = {
  tick: null,
  timeEntries: [],
}

const timerReducer = (state, { type, payload }) => {
  switch (type) {
    case actionTypes.tick:
      return state
    case actionTypes.play:
      return state
    case actionTypes.pause:
      return state
    case actionTypes.stop:
      return state
    default:
      throw new Error(`Unhandled type: ${type}`)
  }
}

const useTimer = () => {
  const [state, dispatch] = useReducer(timerReducer, initialState)

  return {}
}

const Timer = () => {
  return null
}

const App = () => {
  return <Timer />
}
```

### The `tick` Action

The `tick` action will be used to re-render the `<Timer/>` component every second. To do this, the component will use the `useInterval` hook as implemented by Dan Abramov in [this blog post](https://overreacted.io/making-setinterval-declarative-with-react-hooks/) . Every second, this action will be fired with "now" (the number of milliseconds since the unix epoch) as its payload. The payload is then assigned to the `tick` property of the timer reducer's state.

```jsx
case actionTypes.tick:
  return { ...state, tick: payload }
```

```jsx
// The number of ms since the unix epoch (a.k.a. "now")
const now = () => new Date().getTime()

const useTimer = () => {
  const [state, dispatch] = useReducer(timerReducer, initialState)

  const tick = () => dispatch({ type: actionTypes.tick, payload: now() })

  return {
    tick,
  }
}

const Timer = () => {
  const { tick } = useTimer()

  useInterval(() => {
    tick()
  }, 1000)

  return null
}
```

### The `play` Action

The `play` action is in charge of starting the timer at "now". Before implementing this action, there are a few utility functions that will need to be defined, though.

First, let's add these functions which will make it easier to deal with a time entry. These will help to create, stop, and easily determine a time entry's "status":

```jsx
// Create a new time entry starting "now" by default
const startTimeEntry = (time = now()) => ({
  startedAt: time,
  elapsedMs: null,
})

// Stop the given time entry at "now" by default
const stopTimeEntry = (timeEntry, time = now()) => ({
  ...timeEntry,
  elapsedMs: time - timeEntry.startedAt,
})

// Return true if a time entry is running, false otherwise
const isTimeEntryRunning = ({ elapsedMs }) => elapsedMs === null

// Return true if a time entry is paused, false otherwise
const isTimeEntryPaused = ({ elapsedMs }) => elapsedMs !== null
```

Next, let's define some more utility functions, but this time to help derive information from the `useTimer` hook state (a.k.a. "selectors"):

```jsx
// Get the current time entry, which is always the latest one
const getCurrTimeEntry = (state) =>
  state.timeEntries[state.timeEntries.length - 1]

// Return true if the timer is stopped, false otherwise
const isStopped = (state) => state.timeEntries.length === 0

// Return true if the timer is running, false otherwise
const isRunning = (state) =>
  state.timeEntries.length > 0 && isTimeEntryRunning(getCurrTimeEntry(state))

// Return true if the timer is paused, false otherwise
const isPaused = (state) =>
  state.timeEntries.length > 0 && isTimeEntryPaused(getCurrTimeEntry(state))

// Return the total number of elapsed ms
const getElapsedMs = (state) => {
  if (isStopped(state)) return 0

  return state.timeEntries.reduce(
    (acc, timeEntry) =>
      isTimeEntryPaused(timeEntry)
        ? acc + timeEntry.elapsedMs
        : acc + (now() - timeEntry.startedAt),
    0
  )
}
```

These methods will make it easy to know what is the current time entry, if the timer is running, paused, or stopped, and how much time has passed since it was started.

Alright, those were a lot of utility functions! Let's focus in the `play` action implementation:

```jsx
case actionTypes.play:
  if (isRunning(state)) return state

  return {
    ...state,
    timeEntries: state.timeEntries.concat(startTimeEntry(payload)),
  }
```

The `play` action can only be executed if the timer isn't currently running, thus the state is returned as it's unless that's the case. Otherwise, a new time entry is "started" (created) and added to the list of time entries.

### The `pause` Action

The `pause` action can only be executed if timer is running. It will find the currently running time entry (the last one), and compute the number of elapsed milliseconds since it started until now (i.e., how much time it ran for). Here's the implementation:

```jsx
case actionTypes.pause:
  if (isStopped(state)) return state
  if (isPaused(state)) return state

  const currTimeEntry = getCurrTimeEntry(state)
  return {
    ...state,
    timeEntries: state.timeEntries
      .slice(0, -1)
      .concat(stopTimeEntry(currTimeEntry)),
  }
```

### The `stop` Action

The `stop` action removes all the existing time entries to stop the timer and can be executed at any time. Its implementation is straightforward:

```jsx
case actionTypes.stop:
  return { ...state, timeEntries: [] }
```

## The `useTimer` Hook

Now that the timer reducer has been implemented, the `useTimer` hook will expose its API to consumers as follows:

```jsx
const useTimer = () => {
  const [state, dispatch] = useReducer(timerReducer, initialState)

  const pause = () => dispatch({ type: actionTypes.pause, payload: now() })
  const play = () => dispatch({ type: actionTypes.play, payload: now() })
  const stop = () => dispatch({ type: actionTypes.stop })
  const tick = () => dispatch({ type: actionTypes.tick, payload: now() })

  const running = isRunning(state)
  const elapsedMs = getElapsedMs(state)

  return {
    pause,
    play,
    running,
    stop,
    tick,
    elapsedMs,
  }
}
```

The `useTimer` consumer is the `<Timer/>` component, and its implementation could look like this (very simplified and with no styles whatsoever for brevity):

```jsx
const Timer = () => {
  const { pause, play, running, stop, tick, elapsedMs } = useTimer()

  const zeroPad = (x) => (x > 9 ? x : `0${x}`)
  const seconds = Math.floor((elapsedMs / 1000) % 60)
  const minutes = Math.floor((elapsedMs / (1000 * 60)) % 60)
  const hours = Math.floor((elapsedMs / (1000 * 60 * 60)) % 24)

  useInterval(() => {
    tick()
  }, 1000)

  return (
    <div>
      <p>
        {zeroPad(hours)}:{zeroPad(minutes)}:{zeroPad(seconds)}
      </p>
      {running ? (
        <button onClick={pause}>pause</button>
      ) : (
        <button onClick={play}>play</button>
      )}
      <button onClick={stop}>stop</button>
    </div>
  )
}
```

## Conclusion 🤝

Alright, that was a bit longer than I anticipated. The idea of using time entries to store the timer's state can be extended to include more information in each time entry, and thus be able to answer questions such as the ones I posted in the introduction. There's a [CodeSandbox demo](https://codesandbox.io/s/react-timer-app-siyh4) of the `<Timer/>` component and also a [GitHub repo](https://github.com/diegocasmo/react-timer-app) with all the code needed. Post a comment below if you have a question or idea to share 🙂.
