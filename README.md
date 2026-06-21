# GraphSphere Mobile App

GraphSphere is an offline-first knowledge management application built with Flutter. It combines graph-based note organization with AI-powered insights, ensuring a seamless, lag-free experience whether connected to a server or completely offline.

> 🔒 **Repository Status: Architectural Showcase**
> *Please note: The full source code for GraphSphere is currently closed-source as it is an active graduation project and contains proprietary AI and synchronization logic. This repository serves as an architectural overview to demonstrate the Clean Architecture implementation, BLoC state management, and offline-first engineering principles used to build the application.*

## Tech Stack

* **Flutter**: Core framework for cross-platform mobile development.
* **BLoC / Cubit**: Reactive state management for predictable UI behavior and strict logic separation.
* **Clean Architecture**: Feature-driven separation of concerns across Data, Domain, and Presentation layers.
* **Hive**: High-performance NoSQL local storage for offline data persistence and caching.
* **Dio**: Robust HTTP client for API communication, paired with custom interceptors and offline mutation queues.
* **GetIt**: Dependency injection container for service location and decoupled infrastructure.

## Project Structure

The project is structured entirely around isolated features. Each feature contains its own Data (datasources, models, repositories), Domain (entities, abstract repositories, use cases), and Presentation (BLoC, screens, widgets) layers.

```text
lib/
├── main.dart                                 # App entry point & BlocProviders
│
├── core/                                     # Shared infrastructure & utilities
│   ├── constants/
│   │       constants.dart
│   ├── di/
│   │       injection_container.dart          # GetIt Dependency Injection setup
│   ├── errors/
│   │       failures.dart                     # Global error handling models
│   ├── layout_manager/
│   │       adaptive_layout_manager.dart      # Responsive UI handling
│   ├── models/
│   │       drawer_item_model.dart
│   ├── network/                              # API clients & Offline checks
│   │       api_client.dart
│   │       auth_event_bus.dart
│   │       auth_interceptor.dart             # Token injection & refresh logic
│   │       network_info.dart                 # DNS pre-flight checks
│   ├── routing/
│   │       auth_wrapper.dart                 # Global auth state redirection
│   ├── services/
│   │       storage_service.dart              # Local Hive box management
│   ├── sync/                                 # Offline-first engine
│   │       pending_mutation.dart             # Queued offline actions
│   │       sync_service.dart                 # Background synchronization
│   ├── theme/
│   │       theme.dart
│   │       theme_cubit.dart
│   ├── utils/
│   │       date_formatter.dart
│   │       dio_error_parser.dart             # Centralized API error parsing
│   │       live_markdown_controller.dart
│   │       logger.dart
│   │       responsive_font_size.dart
│   └── widgets/                              # Reusable global UI components
│           action_button.dart
│           app_bottom_nav_bar.dart
│           app_button.dart
│           app_circular_indicator.dart
│           app_content_card.dart
│           app_drawer.dart
│           app_dropdown.dart
│           app_feature_card.dart
│           app_sliver_grid.dart
│           app_status_badge.dart
│           app_step_row.dart
│           app_text_field.dart
│           app_underlined_text_field.dart
│           container_card.dart
│           custom_appbar.dart
│           drawer_menu_list.dart
│           drawer_menu_tile.dart
│           empty_state_indicator.dart        # Dynamic empty state views
│           quick_action_button.dart
│           section_header.dart
│           shared_app_bar.dart
│           shared_side_bar.dart              # App-wide navigation drawer
│           skeleton_list_item.dart
│           skeleton_loader.dart
│           theme_toggle_icon.dart
│
└── features/                                 # Independent business logic modules
    ├── agent/                                # AI Assistant module
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       agent_remote_data_source.dart
    │   │   ├── models/
    │   │   │       chat_message_model.dart
    │   │   │       conversation_model.dart
    │   │   └── repositories_implementation/
    │   │           agent_repository_impl.dart
    │   ├── domain/
    │   │   ├── abstract_repositories/
    │   │   │       i_agent_repository.dart
    │   │   ├── entities/
    │   │   │       ai_stream_event.dart
    │   │   │       chat_message.dart
    │   │   │       conversation.dart
    │   │   └── usecases/
    │   │           agent_usecases.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       agent_bloc.dart           # Chat state management
    │       │       agent_event.dart
    │       │       agent_state.dart
    │       └── widgets/
    │           │   ai_chat_body_content.dart
    │           └── agent_widgets/
    │                   ai_suggestion_overlay.dart
    │                   chat_input_area.dart
    │                   chat_message_item.dart
    │                   message_block_renderer.dart
    │                   thought_parser.dart
    │
    ├── assets/                               # Media & attachment handling
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       assets_remote_data_source.dart
    │   │   ├── models/
    │   │   │       asset_model.dart
    │   │   └── repositories_implementation/
    │   │           assets_repository_impl.dart
    │   ├── domain/
    │   │   ├── abstract_repositories/
    │   │   │       i_assets_repository.dart
    │   │   └── entities/
    │   │           asset.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       assets_bloc.dart
    │       │       assets_event.dart
    │       │       assets_state.dart
    │       └── widgets/
    │               asset_preview_dialog.dart
    │               asset_upload_modal.dart
    │
    ├── auth/                                 # Authentication & MFA pipeline
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       auth_local_data_source.dart
    │   │   │       auth_remote_data_source.dart
    │   │   ├── models/
    │   │   │       auth_tokens_model.dart
    │   │   │       two_factor_pending_model.dart
    │   │   │       user_model.dart
    │   │   └── repositories_implementation/
    │   │           auth_repository_impl.dart
    │   ├── domain/
    │   │   ├── abstract_repositories/
    │   │   │       i_auth_repository.dart
    │   │   ├── entities/
    │   │   │       auth_tokens.dart
    │   │   │       two_factor_pending.dart
    │   │   │       user.dart
    │   │   └── usecases/                     # Login, Register, Resend 2FA, etc.
    │   │           get_me_usecase.dart
    │   │           login_usecase.dart
    │   │           logout_usecase.dart
    │   │           register_usecase.dart
    │   │           resend_2fa_usecase.dart
    │   │           verify_2fa_usecase.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       auth_bloc.dart            # Core auth state machine
    │       │       auth_event.dart
    │       │       auth_state.dart
    │       ├── screens/
    │       │       login_screen.dart
    │       │       register_screen.dart
    │       │       verify_screen.dart
    │       │       welcome_screen.dart
    │       └── widgets/
    │           ├── login_widgets/
    │           │       auth_app_bar.dart
    │           │       login_body_content.dart
    │           │       login_desktop_layout.dart
    │           │       login_mobile_layout.dart
    │           │       login_tablet_layout.dart
    │           ├── register_widgets/
    │           │       register_body_content.dart
    │           │       register_desktop_layout.dart
    │           │       register_mobile_layout.dart
    │           │       register_tablet_layout.dart
    │           ├── verify_widgets/
    │           │       verify_body_content.dart
    │           │       verify_desktop_layout.dart
    │           │       verify_mobile_layout.dart
    │           │       verify_tablet_layout.dart
    │           └── welcome_widgets/
    │                   welcome_body_content.dart
    │                   welcome_desktop_layout.dart
    │                   welcome_mobile_layout.dart
    │                   welcome_screen_app_bar.dart
    │                   welcome_tablet_layout.dart
    │
    ├── calendar/                             # Calendar view (Coming Soon)
    │   └── presentation/
    │       └── widgets/
    │               calendar_body_content.dart
    │
    ├── dashboard/                            # Main routing shell
    │   └── presentation/
    │       ├── bloc/
    │       │       navigation_bloc.dart      # Sidebar routing state
    │       │       navigation_event.dart
    │       │       navigation_state.dart
    │       ├── screens/
    │       │       dashboard_screen.dart     # IndexedStack container
    │       └── widgets/
    │           └── dashboard_widgets/
    │                   dashboard_desktop_layout.dart
    │                   dashboard_mobile_layout.dart
    │                   dashboard_tablet_layout.dart
    │
    ├── flashcards/                           # Spaced repetition learning
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       flashcards_local_data_source.dart
    │   │   │       flashcards_remote_data_source.dart
    │   │   ├── models/
    │   │   │       flashcard_card_model.dart
    │   │   │       flashcard_set_model.dart
    │   │   └── repositories_implementation/
    │   │           flashcards_repository_impl.dart
    │   ├── domain/
    │   │   ├── entities/
    │   │   │       flashcard_card.dart
    │   │   │       flashcard_set.dart
    │   │   ├── repositories/
    │   │   │       flashcards_repository.dart
    │   │   └── usecases/
    │   │           flashcard_usecases.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       flashcard_detail_bloc.dart
    │       │       flashcard_sets_bloc.dart
    │       ├── screens/
    │       │       flashcard_edit_screen.dart
    │       │       flashcard_quiz_screen.dart
    │       └── widgets/
    │               create_flashcard_set_dialog.dart
    │
    ├── graph/                                # Knowledge Graph visualization
    │   └── presentation/
    │       └── widgets/
    │           └── graph_widgets/
    │                   graph_body_content.dart
    │                   graph_filter_controls.dart
    │                   graph_node_widget.dart
    │                   graph_zoom_controls.dart
    │
    ├── help/
    │   └── presentation/
    │       └── widgets/
    │           └── help_widgets/
    │                   guide_body_content.dart
    │
    ├── home/                                 # Dashboard landing view
    │   └── presentation/
    │       ├── home_widgets/
    │       │       home_body_content.dart
    │       └── widgets/
    │               notebook_card.dart
    │               recent_note_card.dart
    │
    ├── notebooks/                            # Folder structure & organization
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       notebooks_local_data_source.dart
    │   │   │       notebooks_remote_data_source.dart
    │   │   ├── models/
    │   │   │       folder_model.dart
    │   │   │       notebook_model.dart
    │   │   └── repositories_implementation/
    │   │           notebooks_repository_impl.dart
    │   ├── domain/
    │   │   ├── abstract_repositories/
    │   │   │       i_notebooks_repository.dart
    │   │   ├── entities/
    │   │   │       folder.dart
    │   │   │       notebook.dart
    │   │   └── usecases/
    │   │           create_notebook_usecase.dart
    │   │           get_cached_notebooks_usecase.dart
    │   │           get_notebooks_usecase.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       notebook_bloc.dart
    │       │       notebook_event.dart
    │       │       notebook_state.dart
    │       └── widgets/
    │           ├── dialogs/
    │           │       create_folder_dialog.dart
    │           │       create_notebook_dialog.dart
    │           └── tree_nodes/               # Recursive file tree UI
    │                   asset_tile.dart
    │                   flashcard_set_tile.dart
    │                   folder_tile.dart
    │                   notebook_tree_node_tile.dart
    │                   note_tile.dart
    │
    ├── notes/                                # Core note-taking & markdown
    │   ├── data/
    │   │   ├── datasources/
    │   │   │       notes_local_data_source.dart  # Hive cache
    │   │   │       notes_remote_data_source.dart # API fetching
    │   │   ├── models/
    │   │   │       note_components_model.dart
    │   │   │       note_graph_model.dart
    │   │   │       note_model.dart
    │   │   │       search_results_model.dart
    │   │   └── repositories_implementation/
    │   │           notes_repository_impl.dart    # Smart Merge
    │   ├── domain/
    │   │   ├── abstract_repositories/
    │   │   │       i_notes_repository.dart
    │   │   ├── entities/
    │   │   │       note.dart
    │   │   │       note_components.dart
    │   │   │       note_graph.dart
    │   │   │       search_results.dart
    │   │   └── usecases/                     # CRUD, RAG context, Hybrid Search
    │   │           create_note_usecase.dart
    │   │           delete_note_usecase.dart
    │   │           get_cached_notes_usecase.dart
    │   │           get_notes_usecase.dart
    │   │           get_note_by_id_usecase.dart
    │   │           get_note_graph_usecase.dart
    │   │           get_rag_context_usecase.dart
    │   │           hybrid_search_usecase.dart
    │   │           sync_all_notes_usecase.dart
    │   │           sync_offline_queue_usecase.dart
    │   │           update_note_usecase.dart
    │   └── presentation/
    │       ├── bloc/
    │       │       note_crud_bloc.dart       # Mutation operations
    │       │       note_crud_event.dart
    │       │       note_crud_state.dart
    │       │       note_graph_bloc.dart
    │       │       note_graph_event.dart
    │       │       note_graph_state.dart
    │       │       note_search_bloc.dart
    │       │       note_search_event.dart
    │       │       note_search_state.dart
    │       ├── cubit/
    │       │       note_editor_cubit.dart    # Live editor state
    │       │       note_editor_state.dart
    │       ├── screens/
    │       │       note_editor_screen.dart
    │       └── widgets/
    │           ├── notes_widgets/
    │           │       notes_body_content.dart
    │           └── note_editor/
    │                   note_ai_toolbar.dart
    │                   note_breadcrumbs.dart
    │                   note_editor_app_bar.dart
    │                   note_editor_listeners.dart
    │                   note_editor_modals.dart
    │                   note_markdown_editor.dart
    │                   note_markdown_preview.dart
    │
    └── profile/                              # User settings & profile management
        └── presentation/
            ├── screens/
            │       profile_screen.dart
            └── widgets/
                    profile_password_field.dart
                    profile_row.dart
                    profile_section_card.dart
```

## Architectural Highlights

### Strict Feature Layering
Each feature module enforces a strict unidirectional dependency rule:
1.  **Data Layer**: Manages DTOs (`models`), connects to APIs (`datasources`), and implements the repository interfaces.
2.  **Domain Layer**: The purest layer. Contains `entities` (business objects) and `usecases` (business rules). It knows absolutely nothing about HTTP requests, Hive, or Flutter UI.
3.  **Presentation Layer**: The UI layer. Uses **BLoC/Cubit** to convert domain states into visual interfaces. UI widgets remain "dumb," reacting purely to state emissions.

### Bulletproof Offline-First Engine
The `core/sync/` pipeline guarantees high availability. When a user creates or modifies a note:
1. **Pre-Flight Check**: The system uses DNS lookups (`network_info.dart`) to detect "zombie" network states.
2. **Short-Circuit**: If offline, mutations (`POST/PATCH/DELETE`) bypass the remote datasource entirely to prevent `SocketException` crashes.
3. **Queue & Cache**: The payload is instantly written to the local Hive cache and added to the `PendingMutation` queue.
4. **Background Sync**: Once connectivity returns, `sync_service.dart` drains the queue, resolving local UUIDs with server-generated IDs seamlessly.
