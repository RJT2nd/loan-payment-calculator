<template>
    <div class="payment-calculator">
        <v-form>
            <v-container>
                <v-flex xs12 sm6 md3>
                    <v-text-field label="loan amount" v-model="loanAmount"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md3>
                    <v-text-field label="interest rate" v-model="interestRate"></v-text-field>
                </v-flex>
                <v-flex xs12 sm6 md3>
                    <v-text-field label="amount of payments" v-model="payments"></v-text-field>
                </v-flex>
                <v-btn @click="calculate">Calculate</v-btn>
            </v-container>
        </v-form>
        <v-text-field label="Payment" v-model="payment"></v-text-field>
        <svg></svg>
        <table>
            <tr>
                <td><b>Month</b></td>
                <td><b>Payment</b></td>
                <td><b>Balance</b></td>
                <td><b>Equity</b></td>
            </tr>
            <tr v-for="(balance, index) in balance" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ Math.round(payment * 100) / 100 }}</td>
                <!-- B = L[(1 + c)n - (1 + c)p]/[(1 + c)n - 1] -->
                <td>{{ Math.round(balance * 100) / 100 }}</td>
                <td>{{ Math.round((loanAmount - balance) * 100) / 100 }}</td>
            </tr>
        </table>
    </div>
</template>

<script>
import d3 from 'd3';

export default {
    data() {
        return {
            loanAmount: 0,
            interestRate: 0,
            payments: 0,
            payment: 0,
            balance: []
        }
    },
    methods: {
        calculate: function() {
            //P = L[c(1 + c)n]/[(1 + c)n - 1]
            var L = this.loanAmount;
            var c = this.interestRate / 12;
            var n = this.payments;
            this.payment = L * (c * Math.pow(1 + c, n))/(Math.pow(1 + c, n) - 1);
            
            for(var p = 1; p <= n; p++) {
                // B = L[(1 + c)n - (1 + c)p]/[(1 + c)n - 1]
                var balance = L * (Math.pow(1 + c, n) - Math.pow(1 + c, p)) / (Math.pow(1 + c, n) - 1);
                this.balance.push(balance);
            }
        },
        drawChart: function(data) {
            var svgWidth = 600, svgHeight = 400;
            var margin = { top: 20, right: 20, bottom: 30, left: 50 };
            var width = svgWidth - margin.left - margin.right;
            var height = svgHeight - margin.top - margin.bottom;
            var svg = d3.select('svg')
                .attr("width", svgWidth)
                .attr("height", svgHeight);
        },
        parseData: function(data) {
           // for(int i = 0; i < data.length; i++) {
                
            //}
        }
    }
}

</script>

<style>

.line-chart
{
    border: 1px solid lightgray;
}

</style>