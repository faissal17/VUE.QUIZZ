<script setup>
import { ref, computed } from "vue";
import "./assets/main.css";

var incorrectAnswers = [];
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
    just: "The ability to launch instances on demand when needed allows users to launch and terminate instances in response to a varying workload. This is a more economical practice than purchasing enough on-premises servers to handle the peak load",
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
    just: "AWS DMS helps users migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. AWS DMS can migrate data to and from most widely used commercial and open-source databases",
    selected: null,
  },
  {
    question:
      "Which AWS offering enables users to find, buy, and immediately start using software solutions in theirAWS environment?",
    answer: 3,
    options: ["AWS Config", "AWS OpsWorks", "AWS SDK", "AWS Marketplace"],
    just: "AWS Marketplace is a digital catalog with thousands of software listings from independent software vendors that makes it easy to find, test, buy, and deploy software that runs on AWS to applications that rely on the database. AWS DMS can migrate data to and from most widely used commercial and open-source databases",
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
    just: "Amazon VPC lets users provision a logically isolated section of the AWS Cloud where users can launch AWS resources in a virtual network that they define",
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
    just: "Maintaining physical hardware is an AWS responsibility under the AWS shared responsibility model.",
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
    just: "To deliver content to users with lower latency, Amazon CloudFront uses a global network of points of presence (edge locations and regional edge caches) worldwide.",
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
    just: "Multi-factor authentication (MFA) is a simple best practice that adds an extra layer of protection on top of a username and password. With MFA enabled, when a user signs in to an AWS Management Console, they will be prompted for their username and password (the first factor—what they know), as well as for an authentication code from their MFA device (the second factor—what they have). Taken together, these multiple factors provide increased security for AWS account settings and resources.",
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
    just: "AWS CloudTrail helps users enable governance, compliance, and operational and risk auditing of their AWS accounts. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail. Events include actions taken in the AWS Management Console, AWS Command Line Interface (CLI), and AWS SDKs and APIs.",
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
    just: "Amazon SNS and Amazon CloudWatch are integrated so users can collect, view, and analyze metrics for every active SNS. Once users have configured CloudWatch for Amazon SNS, they can gain better insight into the performance of their Amazon SNS topics, push notifications, and SMS deliveries",
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
    just: "The AWS Acceptable Use Policy provides information regarding prohibited actions on the AWS infrastructure.",
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
    } else if (q.answer != q.selected) {
      incorrectAnswers.push(q);
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
      inputError: "",
    };
  },
  methods: {
    toggle() {
      if (this.name) {
        this.show = !this.show;
      } else if (!this.name) {
        this.inputError = "This field is required.";
      } else {
        this.inputError = "";
      }
    },
    onKeyDown(event) {
      if ((event.ctrlKey || event.metaKey) && event.keyCode === 82) {
        event.preventDefault();
      }
    },
    reloadPage() {
      window.location.reload();
    },
  },
  mounted() {
    document.addEventListener("keydown", this.onKeyDown);
  },
};
</script>

<template>
  <!-- div centred -->
  <div v-if="show" class="division">
    <input
      class="inp"
      type="text"
      name="name"
      id="name"
      v-model="name"
      placeholder="Enter your name"
    />
    <button type="submit" v-on:click="toggle" class="botn">
      start the quiz
    </button>
    <span v-if="inputError" class="error">{{ inputError }}</span>
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
        <p class="scr">Your score is {{ score }}/{{ questions.length }}</p>
        <div>
          <ul>
            <li v-for="(question, index) in incorrectAnswers" :key="index">
              <p>{{ question.text }}</p>
              <p>The question : {{ question.question }}</p>
              <p class="correct">
                Correct answer : {{ question.options[question.answer] }}
              </p>
              <p class="your">
                Your answer : {{ question.options[question.selected] }}
              </p>
              <p class="just">justification : {{ question.just }}</p>
            </li>
          </ul>
        </div>
        <button class="reload-button" @click="reloadPage">Restart</button>
      </section>
    </mian>
  </div>
</template>
