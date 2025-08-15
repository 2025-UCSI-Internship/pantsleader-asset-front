<script>
  import { onMount } from 'svelte';
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';

  const LS_KEY = 'assets';
  let asset = null;
  let loaded = false;

  $: id = decodeURIComponent($page.params.id || '');

  onMount(() => {
    try {
      const all = JSON.parse(localStorage.getItem(LS_KEY) || '[]');
      asset = all.find(a => String(a.asset_id) === id) || null;
    } catch {
      asset = null;
    } finally {
      loaded = true;
    }
  });

  const fmt = (d) => (d ? new Date(d).toLocaleDateString() : '-');

  function badgeClass(s) {
    if (s === '사용 중') return 'using';
    if (s === '보관 중') return 'stored';
    if (s === '유지보수') return 'maint';
    if (s === '폐기') return 'retired';
    return 'default';
  }

  function diffDays(a, b) {
    if (!a || !b) return null;
    const A = new Date(a), B = new Date(b);
    return Math.ceil((B - A) / (1000 * 60 * 60 * 24));
  }

  $: warranty_days_left = asset?.warranty_end_date
    ? diffDays(new Date(), asset.warranty_end_date)
    : null;

  function removeAsset() {
    if (!asset) return;
    if (!confirm('정말 이 자산을 삭제할까요?')) return;
    try {
      const all = JSON.parse(localStorage.getItem(LS_KEY) || '[]');
      const next = all.filter(a => a.asset_id !== asset.asset_id);
      localStorage.setItem(LS_KEY, JSON.stringify(next));
      goto('/assets');
    } catch {
      alert('삭제 중 오류가 발생했습니다.');
    }
  }

  async function copyId() {
    try { await navigator.clipboard.writeText(asset.asset_id); } catch {}
  }
</script>

<section class="container">
  {#if !loaded}
    <div class="card">
      <p>불러오는 중…</p>
    </div>
  {:else if !asset}
    <div class="card">
      <h1>자산을 찾을 수 없습니다</h1>
      <p>경로: <code>{id}</code></p>
      <a class="btn" href="/assets">목록으로</a>
    </div>
  {:else}
    <!-- 페이지 헤더 -->
    <div class="page-header">
      <div>
        <h1>{asset.asset_id}</h1>
        <div class="meta">
          <span class="badge {badgeClass(asset.status)}">{asset.status || '-'}</span>
          <span>{asset.brand || '-'} · {asset.model || '-'}</span>
        </div>
      </div>
      <div class="actions">
        <button class="btn ghost" on:click={copyId}>복사</button>
        <a class="btn ghost" href="/assets">목록</a>
        <button class="btn danger" on:click={removeAsset}>삭제</button>
      </div>
    </div>

    <!-- 정보 카드 -->
    <div class="grid">
      <!-- 식별 -->
      <div class="card">
        <h3>식별</h3>
        <dl>
          <div><dt>태그</dt><dd>{asset.asset_id}</dd></div>
          <div><dt>브랜드</dt><dd>{asset.brand || '-'}</dd></div>
          <div><dt>모델</dt><dd>{asset.model || '-'}</dd></div>
          <div><dt>일련번호</dt><dd>{asset.serial_number || '-'}</dd></div>
          <div><dt>유형/카테고리</dt><dd>{asset.asset_type || '-'} / {asset.asset_category || '-'}</dd></div>
          <div><dt>상태</dt><dd><span class="badge {badgeClass(asset.status)}">{asset.status || '-'}</span></dd></div>
        </dl>
      </div>

      <!-- 구매 · 보증 -->
      <div class="card">
        <h3>구매 · 보증</h3>
        <dl>
          <div><dt>구매일</dt><dd>{fmt(asset.purchase_date)}</dd></div>
          <div><dt>보증 시작</dt><dd>{fmt(asset.warranty_start_date)}</dd></div>
          <div>
            <dt>보증 종료</dt>
            <dd>
              {fmt(asset.warranty_end_date)}
              {#if warranty_days_left !== null}
                {#if warranty_days_left > 0}
                  <span class="pill ok">D-{warranty_days_left}</span>
                {:else if warranty_days_left === 0}
                  <span class="pill warn">오늘 만료</span>
                {:else}
                  <span class="pill bad">{Math.abs(warranty_days_left)}일 경과</span>
                {/if}
              {/if}
            </dd>
          </div>
          <div><dt>공급업체</dt><dd>{asset.supplier || '-'}</dd></div>
          <div><dt>송장 번호</dt><dd>{asset.purchase_order || '-'}</dd></div>
        </dl>
      </div>

      <!-- 위치 · 책임 -->
      <div class="card">
        <h3>위치 · 책임</h3>
        <dl>
          <div><dt>캠퍼스</dt><dd>{asset.campus || '-'}</dd></div>
          <div><dt>위치</dt><dd>{asset.location_room || '-'}</dd></div>
          <div><dt>학부</dt><dd>{asset.department || '-'}</dd></div>
          <div><dt>책임자</dt><dd>{asset.custodian_person || '-'}</dd></div>
        </dl>
      </div>
    </div>
  {/if}
</section>

<style>
  :root{
    --bg:#f5f7fb; --card:#fff; --border:#e6eaf2; --ink:#0e1116;
    --brand:#0a66ff; --brand2:#5aa9ff; --ring:rgba(10,102,255,.18);
    --max:1040px; --radius:14px;
  }
  :global(body){ background:var(--bg); color:var(--ink); }

  .container{ max-width:var(--max); margin:0 auto; padding:20px 16px; }

  .page-header{
    display:flex; justify-content:space-between; align-items:flex-start;
    margin-bottom:20px; flex-wrap:wrap; gap:12px;
  }
  .page-header h1{ margin:0; font-size:1.6rem; }
  .meta{ color:#6c7485; font-size:.92rem; display:flex; gap:8px; align-items:center; flex-wrap:wrap; }

  .grid{ display:grid; gap:16px; grid-template-columns:repeat(auto-fit,minmax(300px,1fr)); }

  .card{
    background:var(--card);
    border:1px solid var(--border);
    border-radius:var(--radius);
    padding:16px;
    box-shadow:0 10px 26px rgba(16,30,55,.06);
  }

  dl{ display:grid; grid-template-columns: 120px 1fr; gap:8px 12px; margin:0; }
  dt{ color:#6c7485; font-size:.9rem; }
  dd{ margin:0; }

  .badge{
    display:inline-block; padding:3px 8px; border-radius:999px; font-size:.86rem;
    border:1px solid #dbe6ff; background:#eef4ff; color:#0a3cff;
  }
  .badge.using{ background:#ecfdf5; border-color:#bbf7d0; color:#047857; }
  .badge.stored{ background:#f1f5f9; border-color:#e2e8f0; color:#334155; }
  .badge.maint{ background:#fff7ed; border-color:#fed7aa; color:#c2410c; }
  .badge.retired{ background:#fef2f2; border-color:#fecaca; color:#b91c1c; }

  .pill{
    display:inline-block; margin-left:8px; padding:2px 8px; border-radius:999px; font-size:.82rem;
  }
  .pill.ok{ background:#ecfdf5; color:#047857; }
  .pill.warn{ background:#fef9c3; color:#92400e; }
  .pill.bad{ background:#fee2e2; color:#b91c1c; }

  .btn{
    padding:8px 14px; border-radius:var(--radius);
    border:1px solid #d8e1f0;
    background:#fff; cursor:pointer; text-decoration:none;
    font-size:.92rem;
  }
  .btn.ghost:hover{ background:#f6f9ff; }
  .btn.danger{ background:#fff5f5; border-color:#ffd7d7; color:#b91c1c; }
</style>
