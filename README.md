# svelte-modal-component

## Installation

1. Ensure you have node installed on your OS (v19 and above - recommended)
2. Navigate to the app where you would like to use the component and run the following in your terminal
```bash
npm install svelte-modal-component --save
```

## How to use the component

1. Inside the script tag of your .svelte file 
```javascript
import { Modal } from 'svelte-modal-component'

let myModal
```
2. Inside an HTML element use the imported Password component like so
```javascript
<Modal bind:this={myModal}></Modal>

```

## Props, handlers, bindings and functions
1. ```bind:this={myModal}``` Required, This binds the modal instance myModal to the component Modal and all its functions and variables that are exported
2. ```{showModal}``` Optional, Boolean, This indicates if the modal myModal is visible or not. If set to true the modal will appear on page load.
3. ```{modalName}``` Optional, String will be ewmited by the exported functions
4. ```{showClose}``` Optional, Boolean to display the x icon on the top right of the modal that can be used to close / cancel the modal with the exported hide() function
5. ```{enableEscClose}``` Optional, Boolean this enables the esc key to close the modal by calling the exported hide() function
6. ```{enableOutsideClick}``` Optional, Boolean this enables the mouse click outside the modal container to close the modal by calling the exported hide() function
7. ```on:openingModal``` Optional, Used for handeling event from modal component, returns the modalName as a string and calls the exported function open()
8. ```on:closingModal``` Optional, Used for handeling event from modal component, returns the modalName as a string and calls the exported function hide()
9. ```open()``` Optional, exported function that opens the modal by setting the showModal to true and emits an event that contains the modalName
10. ```hide()``` Optional, exported function that closes the modal by setting the showModal to false and emits an event that contains the modalName


## Example
```+page.svelte```
``` javascript
    <script>
    import { Modal } from "svelte-modal-component";

    let myModal;
    let modal2;

    let showModal = false;
    let showClose = true;
    let enableEscClose = true;
    let enableOutsideClick = true;
    let modalName = "Test modal";
    let searchInput = "";
    let searchList = [];

    function onOpenLoader() {
        fetch("https://jsonplaceholder.typicode.com/posts")
            .then((response) => response.json())
            .then((json) => {
                console.log(json);
                searchList = json;
            });
    }

    function searchHandler() {
        searchList = [];
        searchInput = "";
        fetch('https://jsonplaceholder.typicode.com/posts/1/comments')
            .then((response) => response.json())
            .then((json) => {
                console.log(json);
                searchList = json;
            });
    }

    function openModalHandler(e) {
        onOpenLoader();
        console.log(e);
    }

    function closeModalHandler(e) {
        searchList = [];
        console.log(e);
    }

    function submitHandler(e) {
        console.log("Submit: ", e);
        console.log("searchList: ", searchList);
        myModal.hide();
    }

    $: filterList = searchList.filter((o) =>
        o["body"].toLowerCase().includes(searchInput.toLowerCase())
    );
</script>

<div>
    <h1>Modal Demo</h1>

    <button on:click={() => myModal.show()}>Show Modal</button>
    <Modal
        bind:this={myModal}
        {showModal}
        {modalName}
        {showClose}
        {enableEscClose}
        {enableOutsideClick}
        on:openingModal={(e) => openModalHandler(e.detail)}
        on:closingModal={(e) => closeModalHandler(e.detail)}
    >
        <div slot="header">
            <h2>Modal Title <span style="padding-left: 100px;"><input type="text" placeholder="Search" bind:value={searchInput} /></span></h2>
        </div>
        <div slot="body">
            <p>Modal content</p>
            {#each filterList as item}
                <p>{item.body}</p>
            {/each}
        </div>
        <div slot="footer">
            <button on:click={searchHandler}>Search</button>
            <button on:click={() => myModal.hide()}>Cancel</button>
            <button on:click={submitHandler}>Submit</button>
        </div>
    </Modal>

    <button on:click={() => modal2.show()}>Show Modal 2</button>
    <Modal bind:this={modal2}></Modal>
</div>

```


## Styling
Can be set with variables associated with every element
```css
--modalBackdropBackgroundColor
--modalBackdropPosition
--modalBackdropWidth
--modalBackdropHeight
--modalBackdropTop
--modalBackdropLeft
--modalBackgroundColor
--modalMaxWidth
--modalHeight
--modalMarginTop
--modalMarginLeft
--modalPadding
--modalBorderRadius
--modalMarginRight
--modalOverflowY
--footerModalHeight
--footerModalPadding
--bodyModalHeight
--bodyModalOverflowY
--headerModalHeight
--closeIconModalWidth
--closeIconModalHeight
--closeIconModalFloat
--closeIconModalCursor
--closeIconModalBorder
--closeIconBackgroundColor
--closeIconBackgroundImage
--closeIconBackgroundRepeat
```

## Feedback and recommendations
Please send me feedback or recommendations for improvements at mariusrossouwcr@gmail.com. I would love to here from you. [Donations](https://www.paypal.com/paypalme/MariusFRossouw) are welcome but not necessary.


