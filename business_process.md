```mermaid
flowchart LR

%% Swimlanes (subgraphs)
subgraph L1[Student]
direction TB
  s_start([Start])
  s_openHome[Open site]
  s_choose{Choose feature}
  s_viewUni[Browse universities]
  s_viewRank[View rankings]
  s_viewProg[View programs]
  s_viewEvents[View events]
  s_useCalc[Use scholarship calculator]
  s_useCompare[Compare universities]
  s_contact[Contact university]
  s_end([End])
end

subgraph L2[Web App (UI)]
direction TB
  ui_home[Render Home / Navigation]
  ui_list_unis[Render Universities list]
  ui_uni_detail[Render University detail]
  ui_rankings[Render Rankings]
  ui_programs[Render Programs]
  ui_events[Render Events]
  ui_calc[Render Calculator]
  ui_compare[Render Compare]
  ui_contact_links[Show contact links (website/email/phone)]
end

subgraph L3[App Logic (Services)]
direction TB
  svc_fetch_unis[fetchUniversities]
  svc_get_uni[findUniversityById]
  svc_fetch_rankings[build mockRankings]
  svc_aggregate_programs[aggregate Programs]
  svc_fetch_events[fetch Events]
  svc_calc_score[calculate score + recommendation]
  svc_build_compare[build comparison view]
end

subgraph L4[Data Source (Mock)]
direction TB
  data_unis[mockUniversities]
  data_rankings[mockRankings()]
  data_events[events array]
end

subgraph L5[External University]
direction TB
  ext_site[Open website / email / phone]
end

%% Main navigation
s_start --> s_openHome --> ui_home --> s_choose

%% Universities flow
s_choose -- Universities --> s_viewUni --> ui_list_unis --> svc_fetch_unis --> data_unis
data_unis --> svc_fetch_unis --> ui_list_unis
s_viewUni --> ui_uni_detail
ui_uni_detail --> svc_get_uni --> data_unis
ui_uni_detail --> ui_contact_links --> s_contact --> ext_site --> s_end

%% Rankings flow
s_choose -- Rankings --> s_viewRank --> ui_rankings --> svc_fetch_rankings --> data_unis
svc_fetch_rankings --> data_rankings --> ui_rankings --> s_end

%% Programs flow
s_choose -- Programs --> s_viewProg --> ui_programs --> svc_aggregate_programs --> data_unis
svc_aggregate_programs --> ui_programs --> s_end

%% Events flow
s_choose -- Events --> s_viewEvents --> ui_events --> svc_fetch_events --> data_events
data_events --> ui_events --> s_end

%% Scholarship calculator flow
s_choose -- Calculator --> s_useCalc --> ui_calc --> svc_calc_score --> ui_calc --> s_end

%% Compare flow
s_choose -- Compare --> s_useCompare --> ui_compare --> svc_build_compare --> data_unis
svc_build_compare --> ui_compare --> s_end
```
