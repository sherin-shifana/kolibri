<template>

  <div>
    <BackLinkWithOptions>
      <BackLink
        slot="backlink"
        :to="classRoute('ReportsLessonListPage')"
        :text="$tr('back')"
      />
      <LessonOptionsDropdownMenu
        slot="options"
        optionsFor="report"
        @select="handleSelectOption"
      />
    </BackLinkWithOptions>

    <h1>
      <KLabeledIcon>
        <KIcon slot="icon" lesson />
        {{ lesson.title }}
      </KLabeledIcon>
    </h1>

    <HeaderTable>
      <HeaderTableRow :keyText="coachStrings.$tr('statusLabel')">
        <LessonActive
          slot="value"
          :active="lesson.active"
        />
      </HeaderTableRow>
      <HeaderTableRow :keyText="coachStrings.$tr('recipientsLabel')">
        <Recipients
          slot="value"
          :groupNames="getGroupNames(lesson.groups)"
        />
      </HeaderTableRow>
      <HeaderTableRow
        :keyText="coachStrings.$tr('descriptionLabel')"
        :valueText="lesson.description || $tr('noDescription')"
      />
    </HeaderTable>

    <HeaderTabs>

      <HeaderTab
        :text="coachStrings.$tr('reportLabel')"
        :to="classRoute('ReportsLessonReportPage', {})"
      />
      <HeaderTab
        :text="coachStrings.$tr('learnersLabel')"
        :to="classRoute('ReportsLessonLearnerListPage', {})"
      />
    </HeaderTabs>

  </div>

</template>


<script>

  import commonCoach from '../common';
  import LessonOptionsDropdownMenu from '../plan/LessonSummaryPage/LessonOptionsDropdownMenu';
  import BackLinkWithOptions from '../common/BackLinkWithOptions';

  export default {
    name: 'ReportsLessonHeader',
    components: {
      BackLinkWithOptions,
      LessonOptionsDropdownMenu,
    },
    mixins: [commonCoach],
    computed: {
      lesson() {
        return this.lessonMap[this.$route.params.lessonId];
      },
    },
    methods: {
      handleSelectOption(action) {
        if (action === 'EDIT_DETAILS') {
          this.$router.push(this.$router.getRoute('LessonReportEditDetailsPage'));
        }
        if (action === 'MANAGE_RESOURCES') {
          this.$router.push(
            this.$router.getRoute(
              'SELECTION_ROOT',
              {},
              // So the "X" and "Cancel" buttons return back to the ReportPage
              { last: this.$route.name }
            )
          );
        }
      },
    },
    $trs: {
      back: 'All lessons',
      noDescription: 'No description',
    },
  };

</script>


<style lang="scss" scoped></style>
