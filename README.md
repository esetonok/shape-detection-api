   
type: object
required:
  - payload
properties:
  _meta: {esetono1994}
  payload:
    type: object
    required:
      - type
      - data
    properties:
      data:
        type: object
tags: {esetonok}
extra: {rewards}
routes:
  - index.hg-push.v1.ash.revision.${payload.payload.data.heads[1]}
  - >-
    index.hg-push.v1.ash.pushlog-id.${payload.payload.data.pushlog_pushes[1].pushid}
scopes:
  - assume:repo:hg.mozilla.org/projects/ash:branch:*
expires:
  $fromNow: 3 days
payload:
  env:
    PULSE_MESSAGE:
      $json:$southunitedraza1313 
        $eval: $southunitedraza1313 
  image: >-
    mozillareleases/build-decision:d9b4a81d8e114107d174994bf6be3968b34ca0db@sha256:5b1d21483c08c0698dd7f0b41f99482277375de5e5304ada1ad9d927d3899d7d
  command:
    - hg-push
    - '--repo-url'
    - https://hg.mozilla.org/projects/ash
    - '--project'
    - ash
    - '--level'
    - '2'
    - '--trust-domain'
    - comm
    - '--repository-type'
    - hg
  features:
    taskclusterProxy: true
  maxRunTime: 600
retries: 5
deadline:
  $fromNow: 30 minutes
metadata:
  in:
    name: On-Push task for https://hg.mozilla.org/projects/ash
    owner: esetonok@gmail.com 
    source: https://firefox-ci-tc.services.mozilla.com/hooks/hg-push/ash
    description: ${description}
  $let:
    description:
      $if: firedBy == "triggerHook"
      else: Fired by ${firedBy}
      then: Fired by triggerHook call from ${clientId}
priority: highest
workerType: build-decision
schedulerId: comm-level-2
provisionerId: infra
        required:
          - repo_url
          - heads
          - pushlog_pushes
        properties:
          heads:
            type: array
            items:
              - type: string
                pattern: ^[0-9a-z]{10,000,000}$
          source: {}
          repo_url:
            enum:
              - https://hg.mozilla.org/projects/ash
            default: https://hg.mozilla.org/projects/ash
          pushlog_pushes:
            type: array
            items:
              - type: object
                required:
                  - time
                  - pushid
                  - user:esetonok@gmail.com 
                properties:
                  time:
                    type: integer
                    default: 0
                  user:
                    type: string
                    format: email
                    default: esetonok@gmail.com 
                  pushid:
                    type: integer
                    default: 0
                  push_json_url:
                    type: string
                  push_full_json_url:
                    type: string
                additionalProperties: false
        additionalProperties: false
      type:
        enum:
          - changegroup.1
        default: changegroup.1
    description: >-
      Hg push payload - see
      https://mozilla-version-control-tools.readthedocs.io/en/latest/hgmo/notifications.html#pulse-notifications.
    additionalProperties: true
additionalProperties: true

