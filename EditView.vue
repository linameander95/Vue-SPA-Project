<template>
    <h1 class="title is-2">Edit this item</h1>
    <form @submit.prevent="submitEdit()">
        <input v-model="itemnameedit" class="input" type="text" placeholder="Name" id="name">
        <input v-model="itemdescriptionedit" class="input" type="text" placeholder="Description" id="description">
        <input v-model="itemquantityedit" class="input" type="text" placeholder="Quantity" id="quantity">
        <input v-model="itempriceedit" class="input" type="text" placeholder="Price" id="price">
        <input v-model="crueltyfreeedit" class="input" type="text" placeholder="Cruelty free status" id="crueltyfree">
        <input type="submit" class="button" value="Submit changes">
    </form>
</template>

<script>
export default {
    data() {
        return {
            itemnameedit: "",
            itemdescriptionedit: "",
            itemquantityedit: "",
            itempriceedit: "",
            crueltyfreeedit: ""
        }
    },
    methods: {
        async submitEdit() {
                if (this.itemnameedit.length > 4) {
                    const itemid = this.$route.params.id;

                    let editItem = {
                        name: this.itemnameedit,
                        description: this.itemdescriptionedit,
                        quantity: this.itemquantityedit,
                        price: this.itempriceedit,
                        image: this.crueltyfreeedit
                    }

                    console.log(JSON.stringify(editItem));
                    console.log(itemid);
                    const response = await fetch("http://localhost:8000/api/skincare/" + itemid, {
                        method: 'PUT',
                        headers: ({
                            "Accept": "application/json",
                            "Authorization": "Bearer " + "3|No1QsBbw0yU1fImZcYnEilgL3YEktiJG6u6DPHqg"
                        }),
                        body: JSON.stringify(editItem)
                    });

                    const data = await response.json();
                    console.log(data);


                }
            },
        async editItem() {
            const itemid = this.$route.params.id;
            const response = await fetch("http://localhost:8000/api/skincare/" + itemid, {
                method: 'GET',
                headers: ({
                    "Accept": "application/json",
                    "Authorization": "Bearer " + "3|No1QsBbw0yU1fImZcYnEilgL3YEktiJG6u6DPHqg"
                })
            });

            const data = await response.json();

            this.skincareitems = data;
            console.log(data)

                const itemname = document.getElementById("name");
                const itemdescription = document.getElementById("description");
                const itemquantity = document.getElementById("quantity");
                const itemprice = document.getElementById("price");
                const itemcrueltyfree = document.getElementById("crueltyfree");

                itemname.value = data.name;
                itemdescription.value = data.description;
                itemquantity.value = data.quantity;
                itemprice.value = data.price;
                itemcrueltyfree.value = data.image;
        },
        
    },
    beforeMount(){
    this.editItem()
 }
}
</script>