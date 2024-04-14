<template>
	<div class="toast">
		<div class="toast-content">
			<strong> <i class="nf-oct-verified"></i> {{ message }}</strong>
		</div>
	</div>
</template>

<script>
export default {
	name: "Toast",
	props: {
		message: {
			type: String,
			required: true,
		},
	},
};
</script>

<style scoped>
.toast {
	margin-bottom: 10px;
	animation: pop-in 0.3s ease-in-out;
}

.toast-content {
	font-family: "HurmitNerdFont", sans-serif;
	background-color: #11111186;
	color: #ffffff;
	padding: 15px 25px;
	border-radius: 12px;
	box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
	font-size: 24px;
	font-weight: 400;
	z-index: 9999;
}

@keyframes pop-in {
	0% {
		opacity: 0;
		transform: translateY(20px);
	}
	100% {
		opacity: 1;
		transform: translateY(0);
	}
}
</style>
