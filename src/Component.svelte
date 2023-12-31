<script>
  import { getContext, onMount } from "svelte";
  import { OrgChart } from "d3-org-chart";
  import { icone } from "./icone";

  let compact = true;
  let chart = null;
  let data = [];
  let ret = [];
  let filtre = { noeud: false, actif: false };

  export let dataProvider;
  export let selectionOk = false;
  export let imageOk = false;
  export let colorBg = "yellow";
  export let chpId = "id";
  export let chpParentId = "parentId";
  export let colorTx1 = "#08011E";
  export let colorTx2 = "#716E7B";
  export let colorTx3 = "#716E7B";
  export let chpImage = "image";
  export let chpDivers = "id";
  export let chpNom = "name";
  export let chpPosition = "position";
  export let clickfiche;
  export let initialZoom = 1;

  const { styleable } = getContext("sdk");
  const component = getContext("component");

  function compactOuiNon() {
    if (compact) {
      chart.compact(false).render().fit();
    } else {
      chart.compact(true).render().fit();
    }
    compact = !compact;
  }

  function exportFull() {
    chart.exportImg({ full: true });
  }

  function filterId() {
    if (filtre.actif) {
      // On supprime le filtre
      //filtre.noeud = false;
      filtre.actif = false;
      filtre.noeud[chpParentId] = filtre.memoParent;
      data = dataProvider.rows;
      chart.data(data).render().fit();
    } else {
      // On filtre
      filtre.actif = true;
      ret = [];
      filtre.memoParent = filtre.noeud[chpParentId];
      filtre.noeud[chpParentId] = "";
      ret.push(filtre.noeud);
      cherchefils(filtre.noeud[chpId]);
      chart.data(ret).render().fit();
    }
  }

  function cherchefils(pere) {
    for (let i = 0; i < data.length; i++) {
      if (data[i][chpParentId] == pere) {
        // Je suis le fils de "pere"
        ret.push(data[i]);
        // je cherche mes fils
        cherchefils(data[i][chpId]);
      }
    }
  }

  function filterNom(e) {
    const value = e.srcElement.value;
    chart.clearHighlighting();
    const data = chart.data();
    data.forEach((d) => (d._expanded = false));
    data.forEach((d) => {
      if (value != "" && d.name.toLowerCase().includes(value.toLowerCase())) {
        d._highlighted = true;
        d._expanded = true;
      }
    });
    chart.data(data).render().fit();
    // console.log('filtering chart', e.srcElement.value);
  }

  onMount(async () => {
    data = dataProvider.rows;
    if (imageOk) {
      if (typeof data[0][chpImage] !== "object") imageOk = false;
    }
    chart = new OrgChart()
      .nodeHeight((d) => 85 + (imageOk ? 25 : 0))
      .nodeWidth((d) => 220 + 2)
      .childrenMargin((d) => 50)
      .compactMarginBetween((d) => 35)
      .compactMarginPair((d) => 30)
      .neighbourMargin((a, b) => 20)
      .initialZoom(initialZoom)
      .nodeId((d) => d[chpId])
      .parentNodeId((d) => d[chpParentId])
      .nodeContent(function (d, i, arr, state) {
        let imageDiffVert = 0;
        let mTop = 5;
        let imgDiv = "";
        if (imageOk) {
          imageDiffVert = 25 + 2;
          mTop = 10;
          imgDiv = `<div style="background-color:${colorBg};margin-top:${
            -imageDiffVert - 20
          }px;
          margin-left:15px;border-radius:100px;width:50px;height:50px;" ></div>
          <div style="margin-top:${-imageDiffVert - 20}px;">
            <img src="${
              d.data[chpImage] && d.data[chpImage][0]
                ? d.data[chpImage][0].url
                : icone
            }" style="margin-left:20px;border-radius:100px;width:40px;height:40px;" />
          </div>`;
        }
        return `<div style='width:${d.width}px;height:${
          d.height
        }px;padding-top:${
          imageDiffVert - 2
        }px;padding-left:1px;padding-right:1px'>
          <div style="font-family: 'Inter', sans-serif;background-color:${colorBg}; margin-left:-1px;width:${
            d.width - 2
          }px;height:${
            d.height - imageDiffVert
          }px;border-radius:10px;border: 1px solid #E4E2E9">
            <div style="display:flex;justify-content:flex-end;margin-top:5px;margin-right:8px;color:${colorTx3}">${
              d.data[chpDivers]
            }</div>
            ${imgDiv}
            <div style="font-size:15px;color:${colorTx1};margin-left:20px;margin-top:${mTop}px">  ${
              d.data[chpNom]
            } </div>
            <div style="color:${colorTx2};margin-left:20px;margin-top:3px;font-size:10px;"> ${
              d.data[chpPosition]
            } </div>
          </div>
        </div>`;
      })
      .container(".chart-container")
      .data(data)
      .render()
      .onNodeClick((d) => {
        if (selectionOk) {
          filtre.noeud = d.data;
          filtre.actif = false;
          chart.clearHighlighting();
          chart.setHighlighted(d.data[chpId]).render();
        }
        clickfiche({ fiche: d.data });
      });
  });
</script>

<div use:styleable={$component.styles}>
  <a
    href="view"
    title="Compact/Horizontal"
    style="vertical-align:middle;"
    on:click|preventDefault={compactOuiNon}
  >
    {#if compact}
      <i class="ri-arrow-left-right-fill ri-xl svelte-1ghy1wa"></i>
    {:else}
      <i class="ri-arrow-up-down-fill ri-xl svelte-1ghy1wa"></i>
    {/if}
  </a>

  <a
    href="save"
    title="Export"
    style="vertical-align:middle;"
    on:click|preventDefault={exportFull}
  >
    <i class="ri-save-line ri-xl svelte-1ghy1wa"></i>
  </a>

  {#if selectionOk}
    {#if filtre.noeud === false}
      <i
        class="ri-filter-line ri-xl svelte-1ghy1wa"
        style="vertical-align:middle;color: var(--spectrum-global-color-gray-400);"
      ></i>
    {:else}
      <a
        href="filtre"
        title="Filtre"
        style="vertical-align:middle;"
        on:click|preventDefault={filterId}
      >
        {#if filtre.actif}
          <i class="ri-filter-off-line ri-xl svelte-1ghy1wa"></i>
        {:else}
          <i class="ri-filter-line ri-xl svelte-1ghy1wa"></i>
        {/if}
      </a>
    {/if}
  {/if}

  &nbsp;&nbsp;&nbsp;&nbsp;
  <input
    type="search"
    style="vertical-align:middle;"
    placeholder="Recherche sur le nom"
    on:input={filterNom}
  />

  <div class="chart-container" />
</div>
