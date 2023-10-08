<script lang="ts">
    import Button from "$lib/Button.svelte";
    import {createEventDispatcher} from "svelte";

    const dispatch = createEventDispatcher();

    export let contentInput: string;

    let isPptGenerated: boolean = false;
</script>

<textarea class="section-input" contenteditable="true" bind:value={contentInput}/>

<br>

<div class="operations">
    <Button on:click={() => dispatch("regenerate-content")}>
        Regenerate content
    </Button>
    <Button on:click={() => dispatch("upload-content", contentInput)}>
        Upload content
    </Button>
    <Button on:click={() => dispatch("generate-ppt")}>
        Generate PPT
    </Button>
</div>

<style lang="scss">
    .section-input {
        height: 20rem;
        width: 100%;
        border: 1px solid gray;
        padding: 1rem 1.5rem;

        overflow-y: auto;
    }
</style>