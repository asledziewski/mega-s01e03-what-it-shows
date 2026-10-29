<script lang="ts">
  import raw from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { uniqueById, groupByCause, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';
  const reports = uniqueById(raw as Report[]).filter(r => r.mergedInto === 4471);
  const groups = groupByCause(reports, tenantOf);
  const uncertain = reports.filter(r => notes.oneDayMove.includes(r.id));
  const july = reports.filter(r => r.mergedAt?.slice(0, 10) === '2026-07-02');
  const beforeJuly = reports.filter(r => r.receivedAt.slice(0, 10) < '2026-07-02');
  const tagErrors = reports.filter(r => r.meta.intakeTag !== groups.find(g => g.reports.includes(r))?.cause);
</script>

<p>The export has <strong>{raw.length}</strong> rows and <strong>{reports.length}</strong> distinct reports in #4471. Deduplicating by report ID removes <strong>{raw.length - reports.length}</strong> overlapping export rows. Tenant locale and booking-form version determine the classifier's result; browser locale and intake tags are not ground truth. <strong>{tagErrors.length}</strong> intake tags disagree with that result.</p>

<table>
  <thead><tr><th>Engineering ticket</th><th>Reports</th><th>Scope and evidence</th></tr></thead>
  <tbody>
    {#each groups as g (g.cause)}
      <tr><td>{g.cause === 'parser' ? 'Shared parser reads month first' : 'booking-v3 gives the wrong date hint'}</td><td>{g.reports.length}</td><td>{g.tenants} tenants; {g.firstSeen} through {g.lastSeen}. {g.cause === 'parser' ? 'Day-first tenants using app/email, or booking-v2 forms.' : 'Day-first tenants using booking-v3 forms; hint says MM/DD/YYYY while the form reads the tenant’s order.'}</td></tr>
    {/each}
  </tbody>
</table>

<p>The large merge event on 2026-07-02 grouped <strong>{july.length}</strong> reports; <strong>{beforeJuly.length}</strong> reports had already arrived before that day. A merge timestamp measures desk work, not when a bug began. Reports continue after the merge, through the export's last day. “Known” and “fix queued” do not show that either problem stopped.</p>

<h2>What the classifier cannot prove</h2>
<p><strong>{uncertain.length}</strong> reports are filed under parser by tenant and channel, but their bodies describe a one-day difference, timezone-dependent days, an invite arriving before booking, or a June-only offset. They remain included in the parser count, with no double counting. That count is a routing result, not proof that every report has the same root cause. Do not close these on the strength of a parser fix; investigate timestamps, timezone, original input, booking ID and event history. The evidence does not yet prove one shared third bug.</p>
<table><thead><tr><th>Report</th><th>What the person reported</th></tr></thead><tbody>{#each uncertain as r (r.id)}<tr><td><code>{r.id}</code></td><td>{r.body}</td></tr>{/each}</tbody></table>
