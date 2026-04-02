# Data Visualization in Python

A hands-on project covering the three core visualization libraries in Python's data science ecosystem: Pandas, Matplotlib, and Plotly.

---

## The Big Picture

Raw numbers are hard to reason about. A table of data tells you almost nothing at a glance. A chart of the same data can reveal patterns, outliers, and relationships instantly. That's the entire motivation for this project.

There are three libraries because each solves a different problem:

| Library | When to use it |
|---|---|
| **Pandas `.plot()`** | Quick inspection when your data is already in a DataFrame |
| **Matplotlib** | Full control over styling and layout, raw data (no DataFrame needed) |
| **Plotly** | Interactive charts you can hover, zoom, and share |

Pandas `.plot()` is actually just a shortcut that calls Matplotlib underneath. So the hierarchy is: **Plotly** and **Matplotlib** are the real engines. **Pandas** is a convenience wrapper around Matplotlib.

---

## Environment

- Python 3.9+
- virtualenv named `ex00`
- Libraries: `pandas`, `numpy`, `matplotlib`, `plotly`, `jupyter`

Activate with:
```bash
source ex00/bin/activate
```

---

## Key Concepts to Remember

### Matplotlib works through `plt` and `ax` objects

`plt` is the global interface — good for simple, single plots:
```python
import matplotlib.pyplot as plt
plt.plot(x, y)
plt.show()
```

`ax` is a specific plot area — needed when you have multiple plots or need `twinx()`:
```python
fig, ax1 = plt.subplots()
ax2 = ax1.twinx()  # second y-axis, same x-axis
```

### You don't always need to import Matplotlib explicitly

When you call `df.plot()`, Pandas imports Matplotlib in the background. In Jupyter, plots display automatically — `plt.show()` is optional. Only import `matplotlib.pyplot as plt` when you need to adjust something Pandas doesn't expose (axis limits, tight_layout, etc.).

### Plotly works in traces

A "trace" is one dataset drawn on a figure. You build a figure by adding traces to it:
```python
fig = go.Figure()
fig.add_trace(go.Scatter(...))  # a line
fig.add_trace(go.Box(...))      # a box plot
fig.show()
```

`go.Scatter` → you provide coordinates, Plotly draws them
`go.Box` → you provide raw numbers, Plotly computes the statistics and draws the box

### Plotly has two levels

- **Plotly Express (`px`)** — minimal code, works great with DataFrames
- **Plotly Graph Objects (`go`)** — more verbose, more control, same results

---

## Exercises

### Ex 1 — Pandas Bar Plot
Compares a numerical value across categories (people's names). Each person becomes a bar.

```python
df.plot(kind='bar', x='name', y=['age'], title='Age per name', xlabel='name')
```

### Ex 2 — Pandas Scatter Plot
Shows the relationship between two numerical variables. Each person becomes a dot positioned by age (x) and number of children (y). Older people tend to have more children — the dots drift upward as you move right.

```python
df.plot(kind='scatter', x='age', y='num_children', title='...', color='red')
```

`color=` accepts plain English names (`'red'`, `'blue'`) or hex codes (`'#ff0000'`).

### Ex 3 — Matplotlib Line + Markers
When data isn't in a DataFrame, use Matplotlib directly. You can call `plt.plot()` twice on the same chart — once for the line, once for the dots — each with different styles.

Style shorthand: `'-.r'` = red dashdot line, `'ob'` = blue circles.

```python
plt.plot(x, y, '-.r', linewidth=3)   # red dashdot line
plt.plot(x, y, 'ob', markersize=12)  # blue circles
plt.xlim(1, 8)                        # constrain axis range
plt.ylim(1, 8)
```

### Ex 4 — Matplotlib Twin Axes (`twinx`)
When two variables have very different scales, plot them on separate y-axes sharing the same x-axis. Otherwise the smaller-scale variable gets visually crushed.

```python
fig, ax1 = plt.subplots()
ax1.plot(x, left_data, color='black')
ax2 = ax1.twinx()
ax2.plot(x, right_data, color='red')
```

### Ex 5 — Matplotlib Subplots
Creates a grid of separate plot areas. `plt.subplots(2, 3)` returns a 2D array of `ax` objects. `.flatten()` turns it into a flat list so you can loop through it with a single `for` loop.

```python
fig, axes = plt.subplots(2, 3)
plt.subplots_adjust(hspace=0.5, wspace=0.5)

for i, ax in enumerate(axes.flatten(), start=1):
    ax.set_title(f'Title {i}')
    ax.text(0.5, 0.5, f'(2,3,{i})', ha='center')
```

`enumerate(..., start=1)` gives you both the index `i` and the object `ax` at each step, counting from 1.

### Ex 6 — Plotly Line Chart (two ways)
Same chart built two ways — Plotly Express (short) and Graph Objects (explicit).

```python
# Plotly Express
fig = px.line(df, x='Date', y='Company_A', title='...', labels={...})

# Graph Objects
fig = go.Figure()
fig.add_trace(go.Scatter(x=df['Date'], y=df['Company_A']))
fig.update_layout(title='...', xaxis_title='...', yaxis_title='...')
```

`go.Scatter` defaults to a line. Add `mode='markers'` for dots, `mode='lines+markers'` for both.

### Ex 7 — Plotly Box Plots
A box plot shows the shape of a distribution: median, middle 50%, and range. Useful for comparing multiple distributions side by side. You hand it raw numbers and it computes everything itself.

```python
fig = go.Figure()
fig.add_trace(go.Box(y=y0, name='y0'))
fig.add_trace(go.Box(y=y1, name='y1'))
fig.add_trace(go.Box(y=y2, name='y2'))
fig.update_layout(title='Box plots')
```

---

## What Comes Next

This project introduced the tools. The next step is Pandas for data manipulation — loading real datasets, cleaning them, and then visualizing them with these same libraries.
