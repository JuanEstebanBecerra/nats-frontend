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
		}) // Usa la URL del servidor de NATS

        const sc = StringCodec();

		console.log('Conectado a NATS')

		// Suscribirse a un canal (subject)
		const sub = nc.subscribe('hello') // Cambia 'mi.canal' por el canal que deseas escuchar

		// Manejar los mensajes que lleguen
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
