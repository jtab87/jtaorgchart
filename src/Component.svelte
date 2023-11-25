<script>
  import { getContext, onMount } from "svelte";
  import { OrgChart } from "d3-org-chart";
  import { icone } from "./icone";

  let compact = true;
  let chart = null;
  let data = [];

  export let dataProvider;
  export let rechercheOk = false;
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

  function filterChart(e) {
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
          chart.clearHighlighting();
          chart.setHighlighted(d.data.id).render();
        }
        clickfiche({ fiche: d.data });
      });
  });
</script>

<div use:styleable={$component.styles}>
  <button on:click|preventDefault={compactOuiNon}>
    {#if compact}
      horizontal
    {:else}
      compact
    {/if}
  </button>
  
  {#if rechercheOk}
    &nbsp;&nbsp;&nbsp;&nbsp;
    <input
      type="search"
      placeholder="Recherche sur le nom"
      on:input={filterChart}
    />
  {/if}

  <div class="chart-container" />
</div>
