<script>
    // import { Application as L, Text } from "svelte-pixi";

    import { Application } from "pixi.js";
    import { afterUpdate, onDestroy, onMount } from "svelte";
    import { welcomeMessage } from "./store";
    let pixiContainer;

    let app;

    onMount(() => {
        app = new Application({
            width: 300,
            height: 300,
            backgroundColor: 0xcbf005,
        });

        app.stage.interactiveChildren = false;
        app.stage.sortableChildren = true;
    });

    afterUpdate(() => {
        if (!pixiContainer.contains(app.view)) {
            pixiContainer.appendChild(app.view);
        }
        welcomeMessage.set("Hello Pixi -x- Svelte");
    });

    onDestroy(() => {
        app.destroy(true, true);
    });
</script>

<section>
    <span> {$welcomeMessage}</span>

    <section class="pixi-container" bind:this={pixiContainer} />
</section>
