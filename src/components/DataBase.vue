<template>
    <div>
        <h1>Realtime Database Example</h1>
        <input v-model="newMessage" placeholder="Enter a message" />
        <button @click="addMessage">Add Message</button>

        <p v-if="message">Message: {{ message }}</p>
        <ul>
            <li v-for="(message, key) in messages" :key="key">
                {{ message }}
            </li>
        </ul>
    </div>
</template>

<script>
    import { ref, push, onValue } from "firebase/database";
    import { database } from "@/firebase";

    export default {
        name: "MyDatabase",
        data() {
            return {
                newMessage: "",
                message: "", // ��� ��� �������� ��� "Message"
                messages: {}, // ��� �� ����� ��� ���������
            };
        },
        methods: {
            addMessage() {
                const messagesRef = ref(database, "Messages");
                push(messagesRef, this.newMessage)
                    .then(() => {
                        alert("Message added successfully!");
                        this.newMessage = "";
                    })
                    .catch((error) => {
                        console.error("Error adding message:", error);
                    });
            },
        },
        mounted() {
            // ��� �� message
            const messageRef = ref(database, "message");

            onValue(messageRef, (snapshot) => {
                this.message = snapshot.val() || ""; // ���������� �� ��������� "message"
            });

            // ��� �� ����� ��� ���������
            const messagesRef = ref(database, "Messages");
            onValue(messagesRef, (snapshot) => {
                this.messages = snapshot.val() || {}; // ���������� �� ��������� "messages"
            });
        },
    };
</script>

<style scoped>
    /* Add styles here if necessary */
</style>
