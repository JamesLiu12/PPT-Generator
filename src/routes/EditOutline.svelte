<script lang="ts">
    import Button from "$lib/Button.svelte";
    import {createEventDispatcher} from "svelte";

    const dispatch = createEventDispatcher();

    let outlineInput: string;

    function regenerateOutline() {
        // TODO
    }
</script>

<textarea class="section-input" contenteditable="true" bind:value={outlineInput}/>

<br>

<div class="operations">
    <Button on:click={regenerateOutline}>
        Regenerate outline
    </Button>
    <Button on:click={() => dispatch("generate-content", outlineInput)}>
        Generate content
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