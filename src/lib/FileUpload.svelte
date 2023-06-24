<script lang="ts">
    let files: FileList;

    function changePosition(index: number, direction: number) {
        console.log(index, direction);
        
        const dt = new DataTransfer();
        
        if (files) {
            for (let i = 0; i < files.length; i++) {
                if (i == index) {
                    dt.items.add(files[i + direction]);
                } else if (i == index + direction) {
                    dt.items.add(files[i - direction]);
                } else {
                    dt.items.add(files[i]);
                }
            }
        }
    
        files = dt.files;
    }

    $: console.log(files);
</script>

<input type="file" multiple bind:files />

{#if files}
    {#each Array.from(files) as file, i}
        <div>
            <button class="contrast outline" on:click={() => changePosition(i, -1)} disabled={i == 0}>⬆️</button>
            <button class="contrast outline" on:click={() => changePosition(i, 1)} disabled={i == files.length - 1}>⬇️</button>
            {file.name}
        </div>
    {/each}
{/if}

<style>
    button {
        width: max-content;
        display: inline-block;
    }
</style>
