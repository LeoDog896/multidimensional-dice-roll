<script lang="ts">

  let dimensionAmount = 2;
  $: gravity = [0, -9.8, ...Array(dimensionAmount - 2).fill(0)]

  const possibleBooleanCombinations = (size: number) => {
    const buf = Array(1 << size)
    for (let i = buf.length; i--;) {
      buf[ i ] = Array(size)
      for (let j = size; j--;)
        buf[ i ][ j ] = +!!(i & 1 << j)
    }
    return buf
  }

  $: boolCombinations = possibleBooleanCombinations(dimensionAmount)

  let time = 0;

  // random velocity. Y (2nd dimension) should be positive
  $: velocity = Array(dimensionAmount).fill(0).map((_, i) => (i == 1 ? Math.abs : (a: number) => a)(Math.random() * 10))

  $: position = Array(dimensionAmount).fill(0)
    // apply gravity (1/2at^2)
    .map((it, i) => it + ((1/2) * gravity[i] * (time ** 2)))
    // apply velocity (vot)
    .map((it, i) => it + (velocity[i] * time))
</script>

<input type="range" bind:value={time} min=0 step=0.01 max=10>

<p>{boolCombinations}</p>

<p>Dimensions: <input bind:value={dimensionAmount} type="number" min="2"/></p>
<p>Time: {time}</p>
<p>Velocity: {velocity}</p>
<p>Position: {position}</p>