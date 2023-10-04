<script>
    import Card from "./components/card.svelte";
    import { cardArr as importedCardArr } from "./type.js";
    import { v4 as uuidv4 } from "uuid";

    let newProductName = "";
    let newDescription = "";

    let cardArr = [...importedCardArr];
    let showMessage = false;
    let message = "";

    function showMessageWithTimeout(msg, timeout = 2000) {
        showMessage = true;
        message = msg;
        setTimeout(() => {
            showMessage = false;
            message = "";
        }, timeout);
    }

    function addProduct(event) {
        event.preventDefault();
        const productId = uuidv4();
        if (newProductName.trim() !== "") {
            const newCard = {
                id: productId,
                name: newProductName,
                description: newDescription,
                isDeleted: false, // Khởi tạo 'isDeleted' cho thẻ mới
            };
            cardArr = [...cardArr, newCard];
            newProductName = "";
            newDescription = "";
        }
    }

    function handleEditCard(cardData) {
        console.log("Received card data for edit:", cardData); 
        cardArr = cardArr.map((card) => {
            if (card.id === cardData.id) {
                return cardData;
            } else {
                return card;
            }
        });
        showMessageWithTimeout("Product updated.");
    }

    function handleDeleteCard(event) {
        const { id } = event.detail;
        cardArr = cardArr.filter((card) => card.id !== id);
        showMessageWithTimeout("Product deleted.");
        // console.log("Updated cardArr after delete:", cardArr);
    }
</script>

<div>
    {#if showMessage}
        <div class="message">
            {message}
        </div>
    {/if}
    <h1>Product app</h1>
    <h2>Add product</h2>
    <form id="addProductForm" on:submit={addProduct}>
        <input
            type="text"
            id="productName"
            placeholder="Product Name"
            required
            bind:value={newProductName}
        />
        <input
            type="text"
            id="description"
            placeholder="Description"
            bind:value={newDescription}
        />
        <button type="submit">Add</button>
    </form>
</div>

<div class="main-content">
    <div class="card-list">
        {#each cardArr as card (card.id)}
            <Card
                id={card.id.toString()}
                name={card.name}
                description={card.description}
                on:editCard={handleEditCard}
                on:deleteCard={handleDeleteCard}
            />
        {/each}
    </div>
</div>

<style>
    .message {
        text-align: center;
        color: red;
        size: 25px;
    }
    .main-content {
        align-items: center;
        justify-content: center;
        display: flex;
    }
    .card-list {
        display: block;
        width: 1000px;
    }
    h1,
    h2 {
        text-align: center;
        color: #333;
    }

    form {
        margin-bottom: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    input[type="text"] {
        padding: 5px;
        font-size: 16px;
        border: 1px solid #333;
        border-radius: 3px;
        margin-bottom: 10px;
    }
    button {
        width: 70px;
        padding: 5px 10px;
        text-align: center;
        background-color: #006eff;
        color: white;
        border: none;
        border-radius: 3px;
        cursor: pointer;
        font-size: 16px;
    }
</style>
