<script>
    import { createEventDispatcher } from "svelte";

    const dispatch = createEventDispatcher();
    export let showModal = false;
    export let showClose = true;
    export let enableEscClose = true;
    export let enableOutsideClick = true;
    export let modalName = "";

    export function show() {
        showModal = true;
        dispatch("openingModal", modalName);
    }

    export function hide() {
        showModal = false;
        dispatch("closingModal", modalName);
    }
</script>

<svelte:window
    on:click={(e) => {
        /*
            Closes the Modal when clicked outside the modal content
        */
        if (
            enableOutsideClick === true &&
            e.target.classList[0] === "modal-backdrop" &&
            e.target.id === modalName
        ) {
            hide();
        }
    }}
    on:keydown={(e) => {
        /*
            Closes the Modal on escape key
        */
        if (enableEscClose === true && e.code === "Escape") {
            hide();
        }
        /*
            Prevents Tab key if the Modal is shown
        */
        if (showModal === true && e.code === "Tab") {
            e.preventDefault();
        }
    }}
/>
{#if showModal}
    <div class="modal-backdrop" id={modalName}>
        <div class="modal">
            {#if $$slots.header}
                <div class="header">
                    {#if showClose}
                        <button class="close-icon" on:click={() => hide()} />
                    {/if}
                    <slot name="header" />
                </div>
            {/if}
            {#if $$slots.body}
                <div class="body">
                    <slot name="body" />
                </div>
            {/if}
            {#if $$slots.footer}
                <div class="footer">
                    <slot name="footer" />
                </div>
            {/if}
        </div>
    </div>
{/if}

<style>
    .modal-backdrop {
        background-color: var(--modalBackdropBackgroundColor, rgba(0, 0, 0, 0.6));
        position: var(--modalBackdropPosition, fixed);
        width: var(--modalBackdropWidth, 100%);
        height: var(--modalBackdropHeight, 100%);
        top: var(--modalBackdropTop, 0);
        left: var(--modalBackdropLeft, 0);
    }
    .modal {
        background-color: var(--modalBackgroundColor, white);
        max-width: var(--modalMaxWidth, 80vw);
        height: var(--modalHeight, 454px);
        margin-top: var(--modalMarginTop, 5%);
        margin-left: var(--modalMarginLeft, auto);
        padding: var(--modalPadding, 1rem);
        border-radius: var(--modalBorderRadius, 25px);
        margin-right: var(--modalMarginRight, auto);
        overflow-y: var(--modalOverflowY, none);
    }
    .footer {
        height: var(--footerModalHeight, 15%);
        padding: var(--footerModalPadding, 1rem);
    }
    .body {
        height: var(--bodyModalHeight, 70%);
        overflow-y: var(--bodyModalOverflowY, scroll);
    }
    .header {
        height: var(--headerModalHeight, 15%);
    }
    .close-icon {
        width: var(--closeIconModalWidth, 20px);
        height: var(--closeIconModalHeight, 20px);
        float: var(--closeIconModalFloat, right);
        cursor: var(--closeIconModalCursor, pointer);
        border: var(--closeIconModalBorder, none);
        background-color: var(--closeIconBackgroundColor, transparent);
        background-image: var(
            --closeIconBackgroundImage,
            url("https://raw.githubusercontent.com/MariusRossouw/svelte-password-component/0954bf51d61e9abe8e70359c8cda1cb8f88db105/static/cross.svg")
        );
        background-repeat: var(--closeIconBackgroundRepeat, no-repeat);
    }
</style>
