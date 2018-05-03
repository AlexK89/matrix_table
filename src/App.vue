<template>
	<div id="app">
		<div class="table">
			<div class="table__header">
				<div v-for="title in titles" class="table__header__title">
					<h5>{{ title }}</h5>
				</div>
			</div>
			<div class="table__body__row" v-for="process in dataArray">
				<div class="table__body__row__cell">
					<p>{{process.name}}</p>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.topics" class="topics">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.requirements" class="requirements">{{item.name}},</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.risks">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.controls">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<ul>
						<li v-for="item in process.reminders">{{item.name}}</li>
					</ul>
				</div>
				<div class="table__body__row__cell">
					<p v-if="process.lastAttestation">{{isoToDate(process.lastAttestation.date)}}</p>
				</div>
				<div class="table__body__row__cell">
					<p>{{process.failures}}</p>
				</div>
				<div class="table__body__row__cell">
					<p>{{process.issues}}</p>
				</div>
				<div class="table__body__row__cell">
					<p>{{process.status}}</p>
				</div>
			</div>
		</div>

	</div>
</template>

<script>
	export default {
		name: 'app',
		data() {
			return {
				dataArray: [],
				titles: [
					'Processes',
					'Topics and objectives',
					'Requirements',
					'Risks',
					'Controls',
					'Reminders',
					'Last Attest',
					'Failures',
					'Issues',
					'Status'
				]
			}
		},
		mounted() {
			this.getData();
		},
		methods: {
			isoToDate(date) {
				let ourDate = new Date(date).toDateString();

				return ourDate.substr(ourDate.indexOf(' ') + 1);
			},
			getData() {
				//AJAx request
				axios
					.get('https://demo7244264.mockable.io/matrix')
					.then(response => {
						this.dataArray = response.data;
					})
					.catch((error) => {
						console.log(error);
						setTimeout(() => {
							this.getData();
						}, 2000);
					});
			}
		}
	}
</script>

<style lang="scss" scoped>
	#app {
		width: 90%;
		max-width: 1600px;
		margin: 5em auto 0;
		overflow-x: scroll;
		font-family: 'Roboto', sans-serif;
	}

	.table {
		display: table;

		&__header {
			display: table-row;
			background-color: antiquewhite;

			&__title {
				display: table-cell;
				vertical-align: middle;
				padding: 0.5em 1em;
				font-weight: bold;
			}
		}

		&__body {
			color: #ddd;
			&__row {
				display: table-row;

				&:nth-child(2n) {
					background-color: cornflowerblue;
				}
				&__cell {
					display: table-cell;
					min-width: 100px;
					padding: 0.5em 1em;
					vertical-align: middle;

					ul {
						padding: 0;

						.topics {
							list-style: none;
						}
						.requirements {
							display: inline;
						}
					}
				}
			}
		}
	}
</style>
