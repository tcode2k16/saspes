<template>
    <div id="saspes-hypo-assignment">
        <h3>Hypothetical Assignment</h3>
        <label for="saspes-assignment-effect">Effect of new assignment (currently):</label>
        <input
            id="saspes-assignment-effect"
            v-model.number="assignment.weight"
            type="number"
            min="0"
            max="100"
            value="0"
        >%
        <label for="hypo-grade-select">Grade of new assignment: </label>
        <select
            id="hypo-grade-select"
            v-model="assignment.grade"
        >
            <option
                v-for="grade in gradeOptions"
                :key="grade"
                :value="grade"
            >
                {{ grade }}
            </option>
        </select>
        <br>
        <h4>Your grade with the selected assignment would be {{ hypo.grade }} with a final percent of {{ hypo.fp }}.</h4>
    </div>
</template>
<script>
import { avaliableGrades, fpToGrade, grade_fp } from '../helpers';

export default {
    name: 'HypoAssignment',
    props: {
        currentFP: {
            type: Number,
            required: true
        }
    },
    data: () => ({
        assignment: {
            weight: 0,
            grade: 'B'
        },
        gradeOptions: avaliableGrades
    }),
    computed: {
        hypo() {
            let new_fp = this.assignment.weight * 0.01 * grade_fp(this.assignment.grade) + ((100 - (this.assignment.weight)) * 0.01 * this.currentFP);
            return {
                fp: new_fp.toFixed(2),
                grade: fpToGrade(parseFloat(new_fp.toFixed(2)))
            };
        }
    }
};
</script>
<style lang="less" scoped>
#saspes-hypo-assignment {
    border: 1px solid #CCCCCC;
    border-radius: 4px;
    margin: 10px 20px;
    padding: 0;

    & h3 {
        font-size: 110%;
        margin: 0 10px 10px 10px;
        padding: 0;
        border-top-right-radius: 3px;
        border-top-left-radius: 3px;
    }

    & select {
        margin: 0 auto 0 auto;
        border-radius: 5px 5px 5px 5px;
    }

    & label {
        vertical-align: initial;
        padding-left: 20px;
    }

    & input {
        border-radius: 5px 5px 5px 5px;
        padding: 5px;
    }
}

</style>

