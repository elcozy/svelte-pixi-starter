<script lang="ts">
    import { Application, Assets, Sprite } from "pixi.js";
    import { afterUpdate, onDestroy, onMount } from "svelte";

    export let app: Application;

    let bunny;
    let bunnyRotate;
    let isDragging = false;

    onMount(() => {
        setup();
    });

    afterUpdate(() => {});

    onDestroy(() => {
        if (app) {
            if (bunnyRotate) app?.ticker?.remove(bunnyRotate);
            if (bunny) app.stage?.removeChild(bunny);
        }
    });

    function setup() {
        const stage = app.stage;

        bunny = Sprite.from(Assets.get("bunny"));

        bunny.position.set(0, 0);
        bunny.anchor.set(0);

        stage.addChild(bunny);

        const appWidth = app.renderer.width;
        const appHeight = app.renderer.height;
        bunnyRotate = (delta) => {
            if (isDragging) {
                return;
            }
            // bunny.rotation += 0.1 * delta;
            if (bunny.y < 1 && bunny.x <= appWidth - bunny.width - 1) {
                bunny.x += 2 * delta;
            } else if (
                bunny.x >= appWidth - bunny.x &&
                bunny.y <= appHeight - bunny.height - 2
            ) {
                bunny.y += 2 * delta;
            } else if (bunny.y >= appHeight - bunny.height - 2 && bunny.x > 1) {
                bunny.x -= 2 * delta;
            } else if (bunny.y >= 1 && bunny.x <= 2) {
                bunny.y -= 2 * delta;
            }
        };
        app.view.style.cursor = "pointer";

        app.view.addEventListener("mousedown", () => {
            isDragging = true;
            app.ticker.stop();
        });

        app.view.addEventListener("mouseup", () => {
            isDragging = false;
            app.ticker.start();
        });

        app.ticker.add(bunnyRotate);
    }
</script>
