<script>
    import Modal from "$lib/Modal/Modal.svelte";

    let modal1;
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
        modal1.hide();
    }

    $: filterList = searchList.filter((o) =>
        o["body"].toLowerCase().includes(searchInput.toLowerCase())
    );
</script>

<div>
    <h1>Modal Demo</h1>

    <button on:click={() => modal1.show()}>Show Modal</button>
    <Modal
        bind:this={modal1}
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
            <button on:click={() => modal1.hide()}>Cancel</button>
            <button on:click={submitHandler}>Submit</button>
        </div>
    </Modal>

    <button on:click={() => modal2.show()}>Show Modal 2</button>
    <Modal bind:this={modal2}></Modal>
</div>
