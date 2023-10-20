<script lang="ts">
    import Button from "$lib/Button.svelte";
    import {createEventDispatcher} from "svelte";

    const dispatch = createEventDispatcher();

    let sectionInput = "";
    let sections: Array<string> = [];

    function addSection() {
        if (sectionInput.length == 0) {
            alert("Cannot add empty section!");
            return;
        }

        sections.push(sectionInput);
    }
</script>

<textarea class="section-input" contenteditable="true" bind:value={sectionInput}/>

<br>

<div class="operations">
    <Button on:click={addSection}>
        Add section
    </Button>
    <Button on:click={() => dispatch("generate-outline", sections)}>
        Generate outline
    </Button>
</div>

<style lang="scss">
    .section-input {
        height: 20rem;
        width: 100%;
        border: 1px solid gray;
        padding: 1rem 1.5rem;

        overflow-y: auto;

        font-size: 1.2rem;
        font-family: Georgia, "Times New Roman", serif;
    }
</style>
