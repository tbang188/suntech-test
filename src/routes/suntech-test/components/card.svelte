<script>
    import { createEventDispatcher } from "svelte";

    export let id = "";
    export let name = "";
    export let description = "";

    const dispatch = createEventDispatcher();
    let isEditing = false;
    let editedName = name;
    let editedDescription = description;

    function handleEdit() {
        isEditing = true;
        editedName = name;
        editedDescription = description;
    }

    function handleSave() {
        if (editedName !== name || editedDescription !== description) {
            console.log('Dữ liệu đã thay đổi:');
            console.log('Trước khi chỉnh sửa:', { name, description });
            console.log('Sau khi chỉnh sửa:', { editedName, editedDescription });

            const cardData = { id, name: editedName, description: editedDescription };
            dispatch('editCard', cardData);
        } else {
            console.log('Dữ liệu không thay đổi.');
        }

        isEditing = false;
    }


    function handleDelete() {
        const cardData = { id, name, description };
        dispatch("deleteCard", cardData);
    }
</script>

<div class="card-content">
    {#if isEditing}
        {`ID: ${id}`}<br />
        Tên:<input type="text" bind:value={editedName} /><br>
        Mô tả:<input type="text" bind:value={editedDescription} />
        <div class="button">
            <button class="edit-button" on:click={handleSave}>Save</button>
        </div>
    {:else}
        {`ID: ${id}`}<br />
        {`Tên: ${name}`} <br />
        {`Mô tả: ${description}`}
        <div class="button">
            <button class="edit-button" on:click={handleEdit}>Edit</button>
            <button class="delete-button" on:click={() => handleDelete()}
                >Delete</button
            >
        </div>
    {/if}
</div>

<style>
    input{
        padding: 1px;
        width: 100px;
        font-size: 13px;
        border: 1px solid #333;
        border-radius: 3px;
        margin-bottom: 3px;
    }
    .card-content {
        display: inline-block;
        width: 31%;
        height: 100px;
        border: 2px solid blue;
        border-radius: 4px;
        margin: 5px;
        padding: 4px;
    }
    .button {
        display: flex;
        justify-content: space-around;
    }
    .edit-button,
    .delete-button {
        width: 70px;
        padding: 5px 10px;
        text-align: center;
        background-color: #ff0000;
        color: white;
        border: none;
        border-radius: 3px;
        cursor: pointer;
        font-size: 16px;
    }

    .edit-button {
        background-color: #ffc107;
    }
</style>
