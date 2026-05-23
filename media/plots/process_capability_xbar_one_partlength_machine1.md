
# Filter the data for Machine 1, Temperature 338, and Pressure 200
data_filtered_pc <- X031..1. %>%
  filter(Machine == 1 & Temperature == 338 & Pressure == 200)

# Create xbar.one chart for PartLength
xbar_chart_pc <- qcc(
  data_filtered_pc$PartLength,
  type = "xbar.one",
  title = "Process Capability: X-bar One Chart for Part Length (Machine 1, Temp 338, Pres 200)",
  ylab = "Part Length",
  xlab = "Observation"
)

library(plotly)
plot_data_pc <- data.frame(
  x = 1:length(data_filtered_pc$PartLength),
  y = data_filtered_pc$PartLength,
  CL = xbar_chart_pc$center,
  UCL = xbar_chart_pc$limits[2],
  LCL = xbar_chart_pc$limits[1]
)

p_pc <- plot_ly(plot_data_pc, x = ~x, y = ~y, type = 'scatter', mode = 'lines+markers', name = 'Part Length') %>%
  add_trace(y = ~CL, type = 'scatter', mode = 'lines', name = 'Center Line', line = list(color = '#D55E00')) %>%
  add_trace(y = ~UCL, type = 'scatter', mode = 'lines', name = 'UCL', line = list(color = '#0072B2', dash = 'dash')) %>%
  add_trace(y = ~LCL, type = 'scatter', mode = 'lines', name = 'LCL', line = list(color = '#0072B2', dash = 'dash')) %>%
  layout(title = "Process Capability: X-bar One Chart for Part Length (Machine 1, Temp 338, Pres 200)",
         xaxis = list(title = "Observation"),
         yaxis = list(title = "Part Length", range=c(min(plot_data_pc$LCL, plot_data_pc$y)-1, max(plot_data_pc$UCL, plot_data_pc$y)+1)),
         plot_bgcolor = "#ffffff",
         paper_bgcolor = "#ffffff")

htmlwidgets::saveWidget(as_widget(p_pc), file.path("/content", "project", "media", "plots", "process_capability_xbar_one_partlength_machine1.html"), selfcontained = TRUE)
