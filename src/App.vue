<template>
    <div id="app">
        <h2>{{test}}</h2>
        <div v-for="element in dataArray">
            {{element}}
            <p>==============</p>
        </div>

        <div style="overflow-x:auto;">
            <table border="1">
                <tr>
                    <th>Processes</th>
                    <th>Topics and objectives</th>
                    <th>Requirements</th>
                    <th>Risks</th>
                    <th>Controls</th>
                    <th>Reminders</th>
                    <th>Last Attest</th>
                    <th>Failures</th>
                    <th>Issues</th>
                    <th>Status</th>
                </tr>
                <tr>

                </tr>
                <tr v-for="element in dataArray">
                    <td>{{element.name}}</td>
                    <td>
                        <ul v-for="item in element.topics">
                            <li>{{item.name}}</li>
                        </ul>
                    </td>
                    <td>
                        <p v-for="item in element.requirements">
                            <span>{{item.name}}</span> 
                        </p>
                    </td>
                    <td>
                        <ul v-for="item in element.risks">
                            <li>{{item.name}}</li>
                        </ul>
                    </td>
                    <td>
                        <ul v-for="item in element.controls">
                            <li>{{item.name}}</li>
                        </ul>
                    </td>
                    <td>
                        <ul v-for="item in element.reminders">
                            <li>{{item.name}}</li>
                        </ul>
                    </td>
                    <td v-if="element.lastAttestation">{{isoToDate(element.lastAttestation.date)}}</td>
                    <td v-else></td>
                    <td>{{element.failures}}</td>
                    <td>{{element.issues}}</td>
                    <td>{{element.status}}</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                test: 'hello',
                dataArray: []
            }
        },
        mounted() {
            //AJAx request
            axios
                .get('https://demo7244264.mockable.io/matrix')
                .then(response => {
                    this.dataArray = response.data;
                })
        },
        methods: {
            isoToDate(date) {
                let ourDate = new Date(date).toDateString();

                return ourDate.substr(ourDate.indexOf(' ') + 1);
            }
        }
    }
</script>

<style lang="scss">
    table {
        border-collapse: collapse;
        border-spacing: 0;
        width: 100%;
        border: 1px solid #ddd;
    }

    th, td {
        text-align: left;
        padding: 8px;
    }

    tr:nth-child(even){
        background-color: #f2f2f2
    }
</style>
