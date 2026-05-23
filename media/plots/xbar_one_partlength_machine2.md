
# Filter the data for Machine 2, Temperature 338, and Pressure 200
data_filtered <- X031..1. %>% 
  filter(Machine == 2 & Temperature == 338 & Pressure == 200)

# Create xbar.one chart for PartLength
xbar_chart <- qcc(
  data_filtered$PartLength,
  type = "xbar.one",
  title = "X-bar Chart for Part Length (Machine 2, Temp 338, Pres 200)",
  ylab = "Part Length",
  xlab = "Observation"
)

library(plotly)
plot_data <- data.frame(
  x = 1:length(data_filtered$PartLength),
  y = data_filtered$PartLength,
  CL = xbar_chart$center,
  UCL = xbar_chart$limits[2],
  LCL = xbar_chart$limits[1]
)

p <- plot_ly(plot_data, x = ~x, y = ~y, type = 'scatter', mode = 'lines+markers', name = 'Part Length') %>%
  add_trace(y = ~CL, type = 'scatter', mode = 'lines', name = 'Center Line', line = list(color = '#D55E00')) %>%
  add_trace(y = ~UCL, type = 'scatter', mode = 'lines', name = 'UCL', line = list(color = '#0072B2', dash = 'dash')) %>%
  add_trace(y = ~LCL, type = 'scatter', mode = 'lines', name = 'LCL', line = list(color = '#0072B2', dash = 'dash')) %>%
  layout(title = "X-bar Chart for Part Length (Machine 2, Temp 338, Pres 200)",
         xaxis = list(title = "Observation"),
         yaxis = list(title = "Part Length", range=c(min(plot_data$LCL, plot_data$y)-1, max(plot_data$UCL, plot_data$y)+1)),
         plot_bgcolor = "#ffffff",
         paper_bgcolor = "#ffffff")

htmlwidgets::saveWidget(as_widget(p), file.path("/content", "project", "media", "plots", "xbar_one_partlength_machine2.html"), selfcontained = TRUE)
