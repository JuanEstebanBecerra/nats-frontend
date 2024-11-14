<template>
	<div>
		<button @click="test">test</button>
	</div>
</template>

<script setup lang="ts">
import { connect, StringCodec  } from 'nats.ws'
const test = () => {
	console.log(123)
}

async function connectToNats() {
	try {
		const nc = await connect({
			servers: 'ws://localhost:8080',
			token: 's3cr3t',
		})

        const sc = StringCodec();
		const sub = nc.subscribe('hello')

		;(async () => {
			for await (const msg of sub) {
				console.log(`Mensaje recibido: ${sc.decode(msg.data)}`)
			}
		})()
	} catch (error) {
		console.error('Error al conectar a NATS:', error)
	}
}

connectToNats()
</script>

<style scoped></style>
