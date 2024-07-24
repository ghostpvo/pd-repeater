<script>
  import { getContext } from "svelte"

  export let data
  export let id
  export let title
  export let desciption
  export let label1
  export let additional1
  export let label2
  export let additional2
  export let label3
  export let additional3
  export let label4
  export let additional4

  const { styleable } = getContext("sdk")
  const component = getContext("component")

  $: sortingStatus = {
    order: "ASC",
    sortedBy: "ID",
  }

  $: repeaterData = data.value

  const sortBy = sortingSource => {
    if (sortingStatus.order === "ASC") {
      repeaterData.sort((a, b) => b[sortingSource] - a[sortingSource])
      sortingStatus.order = "DESC"
    } else {
      repeaterData.sort((a, b) => a[sortingSource] - b[sortingSource])
      sortingStatus.order = "ASC"
    }
    console.log(sortingSource)
    repeaterData = [...repeaterData]
  }

  // Expected keys:
  // 'specialRiskValue': Computed field by "likelihood" and "Impact" values and colored by priority
  // 'specialDateTime': UI friendly parsed Date (YYYY.MM.DD HH:mm)
  const isSpecial = function (value, index) {
    if (value === "specialRiskValue") {
      specialRiskValueBuilder(index)
    } else if (value === "specialDateTime") {
      specialDateTimeBuilder(index)
    } else {
      return value
    }
  }

  const specialRiskValueBuilder = function (index) {
    return "Calculation of Impact and Likelihood" + index
  }

  const specialDateTimeBuilder = function (index) {
    const dateString = repeaterData[index].last_edit

    const [datePart, timePart] = dateString.split("T")
    let [year, month, day] = datePart.split("-").map(String)
    let [hours, minutes, seconds] = timePart.split(":").map(String)

    day = day.length < 2 ? "0" + day : day
    month = month.length < 2 ? "0" + month : month

    return `${day}.${month}.${year} ${hours}:${minutes}`
  }
</script>

<div use:styleable={$component.styles}>
  <ul class="sorting-block data-list">
    {#if id}
      <li class="data-column data-column-general">
        <button class="sorting-item" on:click={sortBy(id)}> Sort by </button>
      </li>
    {/if}
    {#if additional1}
      <li class="data-column">
        <button class="sorting-item" on:click={sortBy(additional1)}>
          Sort by
        </button>
      </li>
    {/if}
    {#if additional2}
      <li class="data-column">
        <button class="sorting-item" on:click={sortBy(additional2)}>
          Sort by
        </button>
      </li>
    {/if}
    {#if additional3}
      <li class="data-column">
        <button class="sorting-item" on:click={sortBy(additional3)}>
          Sort by
        </button>
      </li>
    {/if}
    {#if additional4}
      <li class="data-column">
        <button class="sorting-item" on:click={sortBy(additional4)}>
          Sort by
        </button>
      </li>
    {/if}
  </ul>
  <ul class="data-list">
    {#each repeaterData as item}
      <li class="data-item">
        <div class="data-column data-column-general">
          <strong class="data-column-label">#{item[id]}</strong>
          {#if item[title]}
            <h5 class="data-item-title">{item[title]}</h5>
          {/if}
          <span class="data-column-data">{isSpecial(item[desciption])}</span>
        </div>
        {#if additional1}
          <div class="data-column">
            <strong class="data-column-label">{label1}</strong>
            <span class="data-column-data">{isSpecial(item[additional1])}</span>
          </div>
        {/if}
        {#if additional2}
          <div class="data-column">
            <strong class="data-column-label">{label2}</strong>
            <span class="data-column-data">{isSpecial(item[additional2])}</span>
          </div>
        {/if}
        {#if additional3}
          <div class="data-column">
            <strong class="data-column-label">{label3}</strong>
            <span class="data-column-data">{isSpecial(item[additional3])}</span>
          </div>
        {/if}
        {#if additional4}
          <div class="data-column">
            <strong class="data-column-label">{label4}</strong>
            <span class="data-column-data">{isSpecial(item[additional4])}</span>
          </div>
        {/if}
      </li>
    {/each}
  </ul>
</div>

<style>
  .sorting-block {
    display: flex;
    justify-content: space-between;
    list-style: none;
    padding: 0 16px !important;
  }

  .sorting-block .sorting-item {
    display: flex;
    align-items: center;
    padding: 1px 12px;
    font-size: 12px;
    font-weight: 700;
    color: #6e6e6e;
    border: 2px solid #6e6e6e;
    border-radius: 12px;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .sorting-block .sorting-item span {
    display: inline-block;
    margin-right: 5px;
    font-weight: 700;
  }

  .sorting-block .sorting-item:hover {
    color: #fff;
    background-color: #6e6e6e;
  }

  .data-list {
    padding: 0;
    list-style: none;
  }

  .data-list .data-item {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    padding: 16px;
    margin-top: 12px;
    background: #fff;
    box-shadow:
      rgba(0, 0, 0, 0.1) 0px 1px 3px 0px,
      rgba(0, 0, 0, 0.06) 0px 1px 2px 0px;
    box-sizing: border-box;
  }

  .data-list .data-column {
    width: 150px;
  }

  .data-list .data-column-general {
    width: calc(100% - 450px);
  }

  .data-list .data-column-label {
    display: block;
    font-weight: 700;
  }

  .data-list .data-item-title {
    margin: 10px 0 0;
    font-weight: 700;
    font-size: 18px;
  }

  .data-list .data-column-data {
    display: block;
    margin-top: 10px;
    font-size: 16px;
  }
</style>
