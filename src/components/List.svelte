<script>
    import { onMount } from "svelte";
    import { v4 as uuidv4 } from "uuid"; //universally unique ID
    import {items} from "../stores";
    import ToDoApi from "../ToDoApi";
    import Item from "./Item.svelte";
    import NewItem from "./NewItem.svelte";

    function handleNewItem(e) {
        $items = [
            {
                id: uuidv4(),
                text: e.detail,
                completed: false
            },

            ...$items
            

        ];
        ToDoApi.save($items);
    }

    function handleUpdate(e) {
      const index = $items.findIndex((item) => item.id === e.detail.id);
      $items[index] = e.detail;
      ToDoApi.save($items);
    }

    function handleDelete(e) {
        $items = $items.filter((item) => item.id !== e.detail);
        ToDoApi.save($items);
    }

    let itemsSorted = [];

    $: {
        itemsSorted = [...$items];
        itemsSorted.sort((a,b) =>{
            if (a.completed && b.completed) return 0;
            if (a.completed) return 1;
            if (b.completed) return -1; 
                
            
        });
    }

    onMount(async () => {
        $items = await ToDoApi.getAll();

    });
</script>

<style>
    .list {
        padding: 15px;
    }

    .list-status {
        margin: 0;
        text-align: center;
        color: white;
        font-weight: bold;
        font-size: 1.1em;
    }
</style>

<div class="list">
    <NewItem on:newitem={handleNewItem}></NewItem>
    {#each itemsSorted as item (item)}
       <Item {...item} on:update={handleUpdate} on:delete={handleDelete}/>    
    {:else}
        <p class="list-status">No Items Exist</p>
    {/each}    
</div>

