<script lang="ts">
    import {onMount} from "svelte";
    import "../global.scss";
    import EditTeachingMaterial from "./EditTeachingMaterial.svelte";
    import EditOutline from "./EditOutline.svelte";
    import EditContent from "./EditContent.svelte";
    import {error} from "@sveltejs/kit";

    const title = "Teaching Material Generator";

    let teachingMaterial: HTMLElement;
    let outline: string = "outline to show";
    let content: string = "content to show";
    let id: string;
    const httpAddr = "http://localhost:5173/api/0";

    enum Progress {
        EditingTeachingMaterial,
        EditingOutline,
        EditingPptContent,
    }

    let currentPage: Progress = Progress.EditingTeachingMaterial;

    async function sendPostToBackend(jsonToSend: string) {
        const response = await fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });

        if (!response.ok) {
            alert("session timeout!");
            location.reload();
        }

        const result: string = await response.json();
        return result;
    }

    function generateOutline(sections: Array<string>) {
        console.log("Generate outline");

        if (sections.length == 0) {
            alert("Teaching material cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "generate outline",
            "content": sections,
            "client id": id,
        });
        console.log(jsonToSend)

        sendPostToBackend(jsonToSend).then((value) => {outline = value});
        console.log(outline)

        currentPage = Progress.EditingOutline;
    }

    function regenerateOutline() {
        const jsonToSend = JSON.stringify({
            "operation": "regenerate outline",
            "client id": id,
        });

        sendPostToBackend(jsonToSend).then((value) => {outline = value});
        console.log(outline)
    }

    function generateContent(outline: string) {
        console.log("Generate content");

        if (outline.length == 0) {
            alert("Outline cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "generate content",
            "content": outline,
            "client id": id,
        });
        console.log(jsonToSend)

        sendPostToBackend(jsonToSend).then((value) => {content = value});
        console.log(content);

        currentPage = Progress.EditingPptContent;
    }

    function regenerateContent() {
        const jsonToSend = JSON.stringify({
            "operation": "regenerate content",
            "client id": id,
        });

        sendPostToBackend(jsonToSend).then((value) => {content = value});
        console.log(content)
    }

    function uploadContent(content: string) {
        console.log("Upload content");

        if (content.length == 0) {
            alert("Content cannot be empty!");
            return;
        }

        const jsonToSend = JSON.stringify({
            "operation": "upload content",
            "content": content,
            "client id": id,
        });
        console.log(jsonToSend)

        fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        });
    }

    function generatePPT() {
        const jsonToSend = JSON.stringify({
            "operation": "generate ppt",
            "client id": id,
        });

        fetch(httpAddr, {
            method: 'POST',
            headers: new Headers({
                "Content-Type": "text/plain",
                "Content-Length": jsonToSend.length.toString(),
            }),
            body: jsonToSend,
        })
            .then(response => response.blob())
            .then((blob) => {
                let url: string = window.URL.createObjectURL(blob);
                let fileLink: HTMLAnchorElement = document.createElement('a');
                fileLink.href = url;
                fileLink.download = 'generated.pdf';
                document.body.appendChild(fileLink);
                fileLink.click();
                fileLink.remove();
            })
    }

    onMount(async () =>{
        while (true) {
            id = Math.floor(Math.random() * 1000).toString();

            const jsonToSend = JSON.stringify({
                "operation": "check client id",
                "content": id,
            });

            const response = await fetch(httpAddr, {
                method: 'POST',
                headers: new Headers({
                    "Content-Type": "text/plain",
                    "Content-Length": jsonToSend.length.toString(),
                }),
                body: jsonToSend,
            });

            let isIdOk = await response.json()
            console.log(isIdOk)
            if (isIdOk) return;
        }
    });
</script>

<svelte:head>
    <title>{title}</title>
</svelte:head>

<nav>{title}</nav>

<div id="wrapper">
    {#if currentPage === Progress.EditingTeachingMaterial}
        <EditTeachingMaterial on:generate-outline={e => generateOutline(e.detail)}/>
    {:else if currentPage === Progress.EditingOutline}
        <EditOutline on:generate-content={e => generateContent(e.detail)} on:regenerate-outline={e => regenerateOutline()} outlineInput={outline}/>
    {:else}
        <EditContent on:regenerate-content={e => regenerateContent()} on:upload-content={e => uploadContent(e.detail)} on:generate-ppt={e => generatePPT()} contentInput={content} />
    {/if}
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

    :global(.operations) {
        display: flex;
        justify-content: space-around;
    }
</style>