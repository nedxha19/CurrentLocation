<!-- src/routes/+page.svelte -->
<script>
  let message = "Klicke auf »Standort anzeigen«.";
  let rows = [];

  function showPosition(pos) {
    const c = pos.coords;
    rows = [
      ["Latitude", c.latitude],
      ["Longitude", c.longitude],
      ["Accuracy (m)", c.accuracy],
      ["Altitude", c.altitude ?? "–"],
      ["Altitude Accuracy", c.altitudeAccuracy ?? "–"],
      ["Heading (°)", c.heading ?? "–"],
      ["Speed (m/s)", c.speed ?? "–"],
      ["Timestamp", new Date(pos.timestamp).toLocaleString()]
    ];
    message = "";
  }

  function showError(err) {
    rows = [];
    message = "Fehler: " + err.message;
  }

  function getLocation() {
    if (!navigator.geolocation) {
      message = "Geolocation wird nicht unterstützt.";
      return;
    }
    message = "Standort wird ermittelt …";
    navigator.geolocation.getCurrentPosition(showPosition, showError, {
      enableHighAccuracy: true
    });
  }
</script>

<main>
  <section class="box">
    <header>
      <h1>Standort</h1>
      <button on:click={getLocation}>Standort anzeigen</button>
    </header>

    {#if message}
      <p class="msg">{message}</p>
    {:else}
      <table>
        <tbody>
          {#each rows as [k, v]}
            <tr><th>{k}</th><td>{v}</td></tr>
          {/each}
        </tbody>
      </table>
    {/if}
  </section>
</main>

<style>
  :root {
    --bg: #fff7f0;
    --fg: #2a1f14;
    --muted: #9a7b62;
    --box: #ffffff;
    --line: #f2d2b6;
    --brand: #ff7b00;
    --brand-dark: #e76a00;
  }

  body, main { margin: 0; font-family: system-ui, sans-serif; background: var(--bg); color: var(--fg); }
  main { display: flex; justify-content: center; align-items: center; min-height: 100vh; padding: 1rem; }

  .box {
    width: min(600px, 95%);
    background: var(--box);
    border: 1px solid var(--line);
    border-radius: 12px;
    padding: 1rem;
    box-shadow: 0 6px 18px rgba(0,0,0,.08);
  }

  header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem; }
  h1 { margin: 0; font-size: 1.2rem; }

  button {
    background: var(--brand);
    border: none;
    border-radius: 8px;
    color: #fff;
    padding: 0.5rem 1rem;
    font-weight: 600;
    cursor: pointer;
  }
  button:hover { background: var(--brand-dark); }

  .msg {
    color: var(--muted);
    padding: 0.8rem;
    border: 1px dashed var(--line);
    border-radius: 8px;
  }

  table { width: 100%; border-collapse: collapse; }
  th, td { padding: 0.6rem; text-align: left; }
  th { width: 40%; color: var(--muted); font-weight: 600; }
  tr + tr { border-top: 1px solid var(--line); }
</style>

