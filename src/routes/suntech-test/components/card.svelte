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
            console.log("Dữ liệu đã thay đổi:");
            console.log("Trước khi chỉnh sửa:", { name, description });
            console.log("Sau khi chỉnh sửa:", {
                editedName,
                editedDescription,
            });

            const cardData = {
                id,
                name: editedName,
                description: editedDescription,
            };
            dispatch("editCard", cardData);
            name = editedName;
            description = editedDescription;
        } else {
            console.log("Dữ liệu không thay đổi.");
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
        <div class="text-output">{`ID: ${id}`}</div>
        <div class="text-output">
            Tên:<input
                type="text"
                style="width: 249px;margin-left: 19px;"
                bind:value={editedName}
            />
        </div>
        <div class="text-output">
            Mô tả:<input
                type="text"
                style="width: 250px;"
                bind:value={editedDescription}
            />
        </div>
        <div class="button">
            <button class="edit-button" on:click={handleSave}>Save</button>
        </div>
    {:else}
        <div class="text-output">
            {`ID: ${id}`}
        </div>
        <div class="text-output">{`Tên: ${name}`}</div>
        <div class="text-output">{`Mô tả: ${description}`}</div>

        <div class="button">
            <button class="edit-button" on:click={handleEdit}>Edit</button>
            <button class="delete-button" on:click={() => handleDelete()}
                >Delete</button
            >
        </div>
    {/if}
</div>

<style>
    input {
        padding: 0;
        outline: none;
        border: none;
        height: 20px;
        font-size: 13px;
        background-color: rgb(194, 194, 194);
        margin-left: 5px;
    }
    .text-output {
        height: 20px;
        margin-top: 8px;
        margin-bottom: 8px;
    }
    .card-content {
        display: inline-block;
        width: 31%;
        height: 150px;
        border: 2px solid blue;
        border-radius: 4px;
        margin: 5px;
        padding: 4px;
    }
    .button {
        display: flex;
        align-items: flex-end;
        justify-content: space-around;
        margin-top: 15px;
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
