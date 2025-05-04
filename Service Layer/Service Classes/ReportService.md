import pandas as pd

class ReportService:
    def __init__(self, data=None):
        """
        Initializes the ReportService with data.
        
        Args:
            data (list or dict, optional): Data to be used for report generation. Defaults to None.
        """
        self.data = data if data is not None else []
        self.df = pd.DataFrame(self.data)

    def generate_report(self, report_format='csv', file_path='report'):
         """
        Generates a report in the specified format.

        Args:
            report_format (str, optional): Format of the report ('csv' or 'excel'). Defaults to 'csv'.
            file_path (str, optional): Path to save the report. Defaults to 'report'.

        Returns:
            bool: True if report generation and saving was successful, False otherwise.
        """
        try:
            if report_format == 'csv':
                self.df.to_csv(f'{file_path}.csv', index=False)
            elif report_format == 'excel':
                self.df.to_excel(f'{file_path}.xlsx', index=False)
            else:
                raise ValueError("Invalid report format. Choose 'csv' or 'excel'.")
            return True
        except Exception as e:
            print(f"An error occurred: {e}")
            return False

    def add_data(self, new_data):
         """
        Adds new data to the existing data and updates the DataFrame.

        Args:
            new_data (list or dict): New data to add.
        """
        if isinstance(new_data, list):
            self.data.extend(new_data)
        elif isinstance(new_data, dict):
             self.data.append(new_data)
        else:
            raise ValueError("Invalid data format. Expected list or dict.")
        self.df = pd.DataFrame(self.data)

    def get_report_data(self):
        """
        Returns the current report data.

        Returns:
            pd.DataFrame: The current report data as a DataFrame.
        """
        return self.df
