# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**gillilab-lecture-factory** is an automated lecture documentation system that generates comprehensive, production-ready Korean technical guides for YouTube long-form video lectures. The system uses a skill-based approach to create structured, practical learning materials from technical topics.

## Architecture

### Directory Structure

```
gillilab-lecture-factory/
├── .claude/skills/lecture-doc-generator/
│   ├── SKILL.md                    # Skill definition and workflow
│   └── references/
│       ├── template.md              # Standard document template
│       └── section-guidelines.md    # Section-specific writing guides
├── 0001.[주제].md                   # Generated lecture documents
├── 0002.[주제].md
└── ...
```

### Skill-Based Workflow

The core functionality is implemented as a Claude Code skill (`lecture-doc-generator`) that:
1. Accepts a technical topic and optional URLs
2. Researches official documentation using WebSearch/WebFetch
3. Generates a comprehensive Korean lecture document (10,000-20,000 characters)
4. Follows a standardized 9-section structure

### File Naming Convention

Generated lecture documents use a **4-digit zero-padded sequence number** format:

```
[4자리 제로필자리순번].[강의주제].md
```

Examples:
- `0001.FastAPI.md`
- `0002.Docker.md`
- `0003.React Hooks.md`

**Sequence numbering:**
- Check existing files to determine next number
- Start from `0001` if no documents exist
- Increment automatically for each new document

## Generating Lecture Documents

### Using the Skill

Invoke the `lecture-doc-generator` skill with topic information:

```
"생산성 극대화를 위한 터미널 설정 및 사용법" 강의 문서를 작성해줘.
URL: https://example.com/terminal-guide
```

The skill automatically:
- Researches the topic from provided URLs or searches official documentation
- Generates a complete document following the standard template
- Saves as `[next-number].[topic].md` in the repository root

### Document Structure

All generated documents follow this **standardized 9-section structure**:

1. **개요** (Overview) - Definition, features, use cases
2. **시작하기** (Getting Started) - Prerequisites, installation, initial setup
3. **핵심 개념** (Core Concepts) - Fundamental terminology and architecture
4. **필수 도구 설치 및 설정** (Tool Installation) - Step-by-step setup
5. **생산성 향상 플러그인** (Productivity Plugins) - Extensions and enhancements
6. **현대적 CLI 도구** (Modern CLI Tools) - Alternative tools and performance comparisons
7. **실무 예제** (Practical Examples) - 3+ complete, executable examples
8. **베스트 프랙티스** (Best Practices) - Optimization, security, testing, deployment
9. **트러블슈팅** (Troubleshooting) - Common errors and debugging
10. **추가 학습 자료** (Additional Resources) - Official docs, tutorials, community
11. **결론** (Conclusion) - Summary and next steps

### Content Guidelines

**Korean Writing Style:**
- Formal tone (존댓말: "~합니다", "~입니다")
- Technical terms in English with Korean explanations on first use: "컨테이너(Container)"
- Code blocks must specify language: ` ```bash`, ` ```python`, ` ```javascript`

**Code Examples:**
- All examples must be **executable** (copy-paste ready)
- Include complete imports/dependencies
- Add Korean comments for key sections
- Show expected output
- Progress from simple (Hello World) to production-level

**30-Year Full-Stack Developer Perspective:**
- Prioritize practical, real-world scenarios over theory
- Include common mistakes and solutions
- Address performance and security considerations
- Provide production environment tips

## Modifying the Template

The standard template is defined in:
- `.claude/skills/lecture-doc-generator/references/template.md`

Section-specific writing guidelines:
- `.claude/skills/lecture-doc-generator/references/section-guidelines.md`

When modifying these files, ensure:
- Consistency with existing generated documents
- All placeholders use `{variable}` syntax
- Section numbering remains intact

## Skill Workflow (Advanced)

The `lecture-doc-generator` skill follows this 6-step process:

1. **Information Collection** - Gather topic, description, URLs from user
2. **Research** - WebSearch for official docs, WebFetch for content extraction
3. **Structure Generation** - Apply standard template, adjust sections based on topic type
4. **Content Writing** - Write each section following guidelines
5. **File Creation** - Save as `[sequence].[topic].md` in repository root
6. **Review & Iteration** - Offer improvements, accept user feedback

The skill is optimized for:
- Fast initial draft generation (< 15 minutes)
- High-quality, production-ready content
- YouTube long-form lecture suitability
- Developer-focused practical learning

## Key Principles

1. **Always use the skill** - Don't manually create lecture documents; invoke `lecture-doc-generator`
2. **Verify sequence numbers** - Check existing files before creating new documents
3. **Follow naming convention** - 4-digit zero-padded + topic name
4. **Maintain template structure** - All documents use the standard 9-section format
5. **Focus on executability** - Every code example must be copy-paste ready
