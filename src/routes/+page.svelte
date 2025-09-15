<script>
  let status = "Klicke auf „Standort anzeigen“.";
  let rows = [];

  function showPosition(position) {
    const c = position.coords;
    rows = [
      ["Latitude", c.latitude],
      ["Longitude", c.longitude],
      ["Altitude", c.altitude ?? "–"],
      ["Altitude Accuracy", c.altitudeAccuracy ?? "–"],
      ["Accuracy (m)", c.accuracy],
      ["Heading (°)", c.heading ?? "–"],
      ["Speed (m/s)", c.speed ?? "–"],
      ["Timestamp", new Date(position.timestamp).toLocaleString()]
    ];
    status = "";
  }

  function showError(err) { status = "Fehler: " + err.message; }

  function getLocation() {
    if (!("geolocation" in navigator)) { status = "Geolocation wird nicht unterstützt."; return; }
    navigator.geolocation.getCurrentPosition(showPosition, showError, {
      enableHighAccuracy: true, maximumAge: 0, timeout: 10000
    });
  }
</script>

<main class="shell">
  <section class="panel">
    <div class="head">
      <h1>Standort</h1>
      <button class="btn" on:click={getLocation}>Standort anzeigen</button>
    </div>

    {#if status}
      <div class="hint">{status}</div>
    {:else}
      <table class="kv">
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
    --bg: #0f1115; --fg: #e6e7eb; --muted:#9aa0a6;
    --panel:#151923; --line:#242938; --brand:#5b8cff;
  }
  @media (prefers-color-scheme: light) {
    :root { --bg:#f6f7fb; --fg:#0f1115; --muted:#61656f; --panel:#fff; --line:#e6e8ef; --brand:#355eea; }
  }
  * { box-sizing: border-box; }
  html, body, .shell { height: 100%; }
  body { margin: 0; background: var(--bg); color: var(--fg); font: 15px/1.5 ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; }
  .shell { display: grid; place-items: center; padding: 24px; }
  .panel {
    width: min(760px, 94vw);
    background: var(--panel);
    border: 1px solid var(--line);
    border-radius: 16px;
    box-shadow: 0 12px 28px rgba(0,0,0,.22);
    padding: 20px;
  }
  .head { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin-bottom: 12px; }
  h1 { margin: 0; font-size: 18px; font-weight: 700; letter-spacing:.2px; }
  .btn {
    padding: 10px 14px; border: 1px solid transparent; border-radius: 12px;
    background: var(--brand); color: #fff; font-weight: 600; cursor: pointer;
    transition: transform .05s ease, filter .15s ease, box-shadow .2s ease;
    box-shadow: 0 8px 20px rgba(91,140,255,.28);
  }
  .btn:hover { filter: brightness(1.04); }
  .btn:active { transform: translateY(1px); }
  .hint {
    border: 1px dashed var(--line); color: var(--muted);
    padding: 14px; border-radius: 12px; background: transparent;
  }
  .kv {
    width: 100%; border-collapse: collapse; margin-top: 6px; overflow: hidden;
    border: 1px solid var(--line); border-radius: 12px;
  }
  .kv th, .kv td { padding: 12px 14px; text-align: left; }
  .kv th { width: 40%; color: var(--muted); font-weight: 600; }
  .kv tr + tr { border-top: 1px solid var(--line); }
</style>
