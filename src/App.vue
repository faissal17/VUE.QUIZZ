<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question:
      "Why is AWS more economical than traditional data centers for applications with varying compute workloads?",
    answer: 2,
    options: [
      "Amazon EC2 costs are billed on a monthly basis",
      "Users retain full administrative access to their Amazon EC2 instances",
      "Amazon EC2 instances can be launched on demand when needed.",
      "Users can permanently run enough instances to handle peak workloads",
    ],
    selected: null,
  },
  {
    question:
      "Which AWS service would simplify the migration of a database to AWS ?",
    answer: 1,
    options: [
      "AWS Storage Gatewayk",
      "AWS Database Migration Service (AWS DMS)",
      "Amazon EC2",
      "Amazon AppStream 2.0",
    ],
    selected: null,
  },
  {
    question:
      "Which AWS offering enables users to find, buy, and immediately start using software solutions in theirAWS environment?",
    answer: 3,
    options: ["AWS Config", "AWS OpsWorks", "AWS SDK", "AWS Marketplace"],
    selected: null,
  },
  {
    question:
      "Which AWS networking service enables a company to create a virtual network within AWS?",
    answer: 3,
    options: [
      "AWS Config",
      "Amazon Route 53",
      "AWS Direct Connect",
      "Amazon Virtual Private Cloud (Amazon VPC)",
    ],
    selected: null,
  },
  {
    question:
      "Which of the following is an AWS responsibility under the AWS shared responsibility model?",
    answer: 1,
    options: [
      "Configuring third-party applications",
      "Maintaining physical hardware",
      "Securing application access and data",
      "Managing guest operating systems",
    ],
    selected: null,
  },
  {
    question:
      "Which component of the AWS global infrastructure does Amazon CloudFront use to ensure low-latency delivery?",
    answer: 1,
    options: [
      "AWS Regions",
      "Edge locations",
      "Availability Zones",
      "Virtual Private Cloud (VPC)",
    ],
    selected: null,
  },
  {
    question:
      "How would a system administrator add an additional layer of login security to a user's AWS Management Console?",
    answer: 2,
    options: [
      "Use Amazon Cloud Directory",
      "Audit AWS Identity and Access Management (IAM) roles",
      "Enable multi-factor authentication",
      "Enable AWS CloudTrail",
    ],
    selected: null,
  },
  {
    question:
      "Which service can identify the user that made the API call when an Amazon EC2 instance is terminated?",
    answer: 1,
    options: [
      "AWS Trusted Advisor",
      "AWS CloudTrail",
      "AWS X-Ray",
      "AWS Identity and Access Management (AWS IAM)",
    ],
    selected: null,
  },
  {
    question:
      "Which service would be used to send alerts based on Amazon CloudWatch alarms?",
    answer: 3,
    options: [
      "Amazon Simple Notification Service (Amazon SNS)",
      "AWS CloudTrail",
      "AWS Trusted Advisor",
      "Amazon Route 53",
    ],
    selected: null,
  },
  {
    question:
      "Where can a user find information about prohibited actions on the AWS infrastructure?",
    answer: 3,
    options: [
      "AWS Trusted Advisor",
      "AWS Identity and Access Management (IAM)",
      "AWS Billing Console",
      "AWS Acceptable Use Policy",
    ],
    selected: null,
  },
]);
const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      value++;
    }
  });
  return value;
});
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});
const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};
const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  }

  quizCompleted.value = true;
};
</script>
<!-- end of first script -->
<script>
export default {
  data() {
    return {
      show: true,
      name: "",
    };
  },
  methods: {
    toggle() {
      this.show = !this.show;
    },
  },
};
</script>

<template>
  <!-- div centred -->
  <div v-if="show" class="division">
    <input class="inp" type="text" name="name" id="name" v-model="name" />
    <button type="submit" v-on:click="toggle" class="botn">
      start the quiz
    </button>
  </div>
  <!-- div centred -->
  <div v-else>
    <mian class="app">
      <section class="quiz" v-if="!quizCompleted">
        <div class="quiz-info">
          <span class="question">{{ getCurrentQuestion.question }}</span>
        </div>

        <div class="options">
          <label
            v-for="(option, index) in getCurrentQuestion.options"
            :for="'option' + index"
            :class="`option ${
              getCurrentQuestion.selected == index
                ? index == getCurrentQuestion.answer
                  ? 'correct'
                  : 'wrong'
                : ''
            } ${
              getCurrentQuestion.selected != null &&
              index != getCurrentQuestion.selected
                ? 'disabled'
                : ''
            }`"
          >
            <input
              type="radio"
              :id="'option' + index"
              :name="getCurrentQuestion.index"
              :value="index"
              v-model="getCurrentQuestion.selected"
              :disabled="getCurrentQuestion.selected"
              @change="SetAnswer"
            />
            <span>{{ option }}</span>
          </label>
        </div>
        <button
          @click="NextQuestion"
          :disabled="!getCurrentQuestion.selected"
          class="next"
        >
          {{
            getCurrentQuestion.index == questions.length - 1
              ? "Finish"
              : getCurrentQuestion.selected == null
              ? "Select an option"
              : "Next question"
          }}
        </button>
      </section>

      <section v-else>
        <h2>Congratulation, {{ name }} You have finished the quiz!</h2>
        <p>Your score is {{ score }}/{{ questions.length }}</p>
      </section>
    </mian>
  </div>
</template>
