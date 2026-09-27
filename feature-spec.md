# Feature Specification: Image prompts

## Goal

Allow users to include an image when asking the Computing History Agent
a question, so the agent can answer questions about computing-related images.

## User experience

- Add a control beside the message input for attaching one image.
- Accept PNG and JPEG images up to 5 MB.
- After selection, show an image preview and a control to remove it.
- A prompt must contain text, but attaching an image is optional.
- When submitted, display the text and image together in the user's chat message.
- Clear the selected image after a successful submission.
- Image controls must have accessible labels and support keyboard navigation.

## Functional requirements

- Send the prompt text and optional image to the agent using the existing
  OpenAI Responses API integration.
- Extend the existing agent client rather than creating a separate request path for image prompts.
- Preserve the existing behavior for text-only prompts and conversation reset.
- Show a useful error without submitting when the image has an unsupported
  type or exceeds the size limit.
- If submission fails, re-enable the input controls so the user can try again.

## Design constraints

- Match the existing chat interface's visual style.
- Do not add new third-party dependencies unless they are necessary.

## Acceptance criteria

- A user can attach, preview, remove, and replace one valid image.
- Submitting text with an image displays both in the user's chat message.
- The agent response demonstrates that the image was received, such as by
  identifying or describing visible content.
- PNG, JPG, and JPEG files are accepted; unsupported and oversized files
  produce a visible error.
- Text-only prompts continue to work as before.
- After a successful submission or conversation reset, no image remains selected.
