<script lang="ts">
    import "../global.scss";
    import Button from "$lib/Button.svelte";

    const title = "Teaching Material Generator";

    let teachingMaterial: HTMLElement;
    let textsToUpload: Array<string> = [];

    function upload() {
        textsToUpload.push(teachingMaterial.innerHTML);
        console.log("Upload: ", textsToUpload);
    }

    function generateOutline() {
        console.log("Generate");
    }
</script>

<svelte:head>
    <title>{title}</title>
</svelte:head>

<nav>{title}</nav>

<div id="wrapper">
    <div id="teaching-material" contenteditable="true" bind:this={teachingMaterial}/>

    <br>

    <div id="operations">
        <Button on:click={upload}>
            Upload
        </Button>
        <Button on:click={generateOutline}>
            Generate outline
        </Button>
    </div>
</div>


<style lang="scss">
    nav {
        padding: 1rem 2rem;
        background: #eee;
    }
    #wrapper {
        max-width: 40rem; width: 100%;
        margin: 0 auto;
    }
    #teaching-material {
        height: 20rem;
        border: 1px solid gray;
        padding: 1rem 1.5rem;

        overflow-y: auto;
    }

    #operations {
        display: flex;
        justify-content: space-around;
    }
</style>